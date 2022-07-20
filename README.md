-----------------

# flagify: powerful Python toolkit for mark/unmark files and directories 
[![PyPI Latest Release]](https://pypi.org/project/flagify/)

## What is it?

**flagify** is a Python package that provides marking/unmarking files and directores. For years, dealing with processing files, we understood that we need to put flag for files/directories that are already processed. This package provides the ability to mark files/directories and avoid any unnecessary repeatitive processing of same files/directories.
It also can manage serveral processes that willing to write on/in same files/directories. For example, we want to write to a parquet file, the whole processing generate part of the parquet file managed by a separate parallel processes. Using **flagify** avoid writing errors that happen at same time.


## Where to get it
The source code is currently hosted on GitHub at:
https://github.com/MosesDastmard/flagify

Binary installers for the latest released version are available at the [Python
Package Index (PyPI)](https://pypi.org/project/flagify/)

```sh
# or PyPI
pip install flagify
```


## Example

```sh
# or PyPI
from flagify import Flag
import pandas as pd

# lets make a simple csv file
data = {'name':['flagify', 'pandas', 'numpy'],
        'toolkit':['marking', 'data analysis', 'data computing']}

pd.DataFrame(data).to_csv('data.csv', index=False)

class Process(Flag):
    def __init__(self, process_name):
        self.process_name = process_name
        Flag.__init__(self, process_name)
        
    def run(self, csv_path, parquet_path):
        if not self.check_flagged(csv_path):
            pd.read_csv(csv_path).to_parquet(parquet_path)
            self.put_flag(csv_path)
        else:
            print(f"the file {csv_path} is already processed")
```

run it for the first time
```sh
Process(process_name='convert_csv_to_parquet').run('data.csv', 'data.parquet')
```

run it for the second time
```sh
Process(process_name='convert_csv_to_parquet').run('data.csv', 'data.parquet')
```
you will see it avoid to run the same process.


## Another Example

```sh
from flagify import Flag
import pandas as pd
from joblib import Parallel, delayed

# lets make a simple csv file
row = {'name':'flagify',
        'toolkit':'marking'}

def add_row(parquet_path, row):

pd.DataFrame(data).to_csv('data.csv', index=False)

class Process(Flag):
    def __init__(self, process_name):
        self.process_name = process_name
        Flag.__init__(self, process_name)
        
    def run(self, csv_path, parquet_path):
        if not self.check_flagged(csv_path):
            pd.read_csv(csv_path).to_parquet(parquet_path)
            self.put_flag(csv_path)
        else:
            print(f"the file {csv_path} is already processed")
```

run it for the first time
```sh
Process(process_name='convert_csv_to_parquet').run('data.csv', 'data.parquet')
```

run it for the second time
```sh
Process(process_name='convert_csv_to_parquet').run('data.csv', 'data.parquet')
```
you will see it avoid to run the same process.



