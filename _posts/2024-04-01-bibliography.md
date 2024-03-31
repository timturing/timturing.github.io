### Bib 维护

直接维护一个全部文献的bib，在论文中不会引用的entry不会出现

### 去掉多余entry

在compile完之后拿出aux以及这个bib

使用下面这段代码抽取出真正被引用的bib

```python
import re

def extract_cited_keys(aux_file):
    cited_keys = set()
    with open(aux_file, 'r', encoding='utf-8') as aux:
        for line in aux:
            match = re.match(r'\\citation\{(.+?)\}', line)
            if match:
                keys = match.group(1).split(',')
                cited_keys.update(keys)
    return cited_keys

def filter_bib_file(bib_file, cited_keys, output_file):
    with open(bib_file, 'r', encoding='utf-8') as bib:
        with open(output_file, 'w', encoding='utf-8') as output:
            entry = ''
            keep_entry = False
            for line in bib:
                if line.strip().startswith('@'):
                    entry = line.strip() + '\n'
                    key_match = re.match(r'\@(.*?)\{(.+?),', line)
                    if key_match and key_match.group(2) in cited_keys:
                        keep_entry = True
                    else:
                        keep_entry = False
                elif line.strip() == '}':
                    entry += line
                    if keep_entry:
                        output.write(entry)
                    entry = ''
                    keep_entry = False
                elif keep_entry:
                    entry += line
            output.write('\n')

def main():
    aux_file = input("Enter the path to your .aux file: ").strip()
    bib_file = input("Enter the path to your .bib file: ").strip()
    output_file = input("Enter the path for the output .bib file: ").strip()

    cited_keys = extract_cited_keys(aux_file)
    filter_bib_file(bib_file, cited_keys, output_file)

    print("Filtered BibTeX file has been created successfully.")

if __name__ == "__main__":
    main()
```

