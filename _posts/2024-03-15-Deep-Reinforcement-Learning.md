```python
import torch
import torch.nn as nn
import torch.optim as optim
import numpy as np


class PolicyNetwork(nn.Module):
    """
    The Agent or learner, actually is something can can fit a function.
    In the case of Deep Reinforcement Learning, the agent is a neural network.
    It takes the state as input and outputs the probability of taking each action.
    So the input_dim is the dimension of the state and the output_dim is the number of actions.
    Here, we can view both state and action are decided by environment.
    For example, in the grid-world environment, the state is the position of the agent and the action is the movement.
    So the input_dim is the dimension of the position and the output_dim is the number of movements.
    More generally, we are actually in a point in a function, with the number of input_dim variables (e.g. x,y),
    and the actions is the number of directions that we can choose to go (so it's discrete and finite ). 
    """
    def __init__(self, input_dim, output_dim):
        super(PolicyNetwork, self).__init__()
        self.fc = nn.Sequential(
            nn.Linear(input_dim, 128),
            nn.ReLU(),
            nn.Linear(128, output_dim),
            nn.Softmax(dim=-1)
        )

    def forward(self, x):
        return self.fc(x)


def select_action(state):
    """
    Let the learner to decide the action based on the current state.
    This will provide the information about how the learner think of each action through
    the predicted distribution.
    """
    state = torch.from_numpy(state).float().unsqueeze(0)
    probs = policy_network(state)
    action_dist = torch.distributions.Categorical(probs)
    action = action_dist.sample()
    return action.item(), action_dist.log_prob(action)


def update_policy(log_probs, rewards, optimizer, gamma=0.99):
    """
    In a general consideration. The learner have already **tried** to get us from a bad place to a good place.
    Now we are juding whether the learner choosed well.
    In this case, we consider how confident the learner is about the action and how good the action actually is.
    And finally we sum every single move to get the final reward (or negatively, loss).
    The loss is used as feedback for the learner to make it better (just like the supervised learning's label loss).
    """
    discounted_rewards = []
    cumulative_reward = 0
    for r in reversed(rewards):
        cumulative_reward = r + gamma * cumulative_reward
        discounted_rewards.insert(0, cumulative_reward)

    discounted_rewards = torch.tensor(discounted_rewards)
    discounted_rewards = (discounted_rewards - discounted_rewards.mean()) / (discounted_rewards.std() + 1e-9)

    policy_loss = []
    for log_prob, reward in zip(log_probs, discounted_rewards):
        policy_loss.append(-log_prob * reward)

    optimizer.zero_grad()
    policy_loss = torch.stack(policy_loss).sum()
    policy_loss.backward()
    optimizer.step()


# Define the grid-world environment
class GridWorld:
    """
    The environment is the world where the agent lives and interacts with.
    It is decides the state and the reward.
    """
    def __init__(self):
        self.grid_size = 4
        self.state_dim = 2
        self.actions = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # Right, Down, Left, Up
        self.goal = (3, 3)
        self.current_state = (0, 0)

    def reset(self):
        self.current_state = (0, 0)
        return np.array(self.current_state)

    def step(self, action):
        action = self.actions[action]
        next_state = (self.current_state[0] + action[0], self.current_state[1] + action[1])

        if next_state == self.goal:
            reward = 1
            done = True
        else:
            reward = 0
            done = False

        self.current_state = next_state

        return np.array(self.current_state), reward, done


env = GridWorld()
input_dim = env.state_dim
output_dim = len(env.actions)

policy_network = PolicyNetwork(input_dim, output_dim)
optimizer = optim.Adam(policy_network.parameters(), lr=0.01)

max_episodes = 1000

for episode in range(max_episodes): 
    """
    In mathmatical view, the learner is trying to find the best function that can fit the environment.
    So it should be a expectation above on possible action paths.
    However it's impossible to calculate so many paths, so we can only sample some of them.
    """
    state = env.reset()
    rewards = []
    log_probs = []

    done = False
    while not done:
        action, log_prob = select_action(state)
        next_state, reward, done = env.step(action)

        rewards.append(reward)
        log_probs.append(log_prob)

        state = next_state

    update_policy(log_probs, rewards, optimizer)

    if episode % 10 == 0:
        print(f"Episode: {episode}")

print("Training completed.")

# Testing the trained policy
state = env.reset()
done = False
while not done:
    action, _ = select_action(state)
    next_state, reward, done = env.step(action)
    print(f"Current state: {state}, Action: {action}, Reward: {reward}")
    state = next_state

```

