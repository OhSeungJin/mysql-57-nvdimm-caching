# NVDIMM Caching for MySQL 5.7

Optimize MySQL/InnoDB using NVDIMM 

## Build and install

1. Clone the source code: 

```bash
$ git clone https://github.com/meeeejin/mysql-57-nvdimm-caching.git
```

2. Change the value of `BASE_DIR` in the `build.sh` file to the desired value:

```bash
$ vi build.sh
#!/bin/bash

BASE_DIR=/home/xxx/mysql-57-nvdimm-caching
...
```

3. Run the script file:

```bash
$ ./build.sh
```

The above command will compile and build the source code with the default option (i.e., caching new-orders and order-line pages). The available options are:

| Option     | Description |
| :--------- | :---------- |
| --origin   | No caching (Vanilla version)                        |
| --nc-ol    | Caching New-Orders and Order-Line pages (`default`) |
| --nc-st    | Caching New-Orders and Stock pages                  |
| --nc-ol-st | Caching New-Orders, Order-Line and Stock pages      |

If you want the vanilla version, you can run the script as follows:

```bash
$ ./build.sh --origin
```

## Run

1. Add the following three server variables to the `my.cnf` file:

| System Variable                     | Description | 
| :---------------------------------- | :---------- |
| innodb_use_nvdimm_buffer            | Specifies whether to use NVDIMM cache. **true** or **false**. |
| innodb_nvdimm_buffer_pool_size      | The size in bytes of the NVDIMM cache. The default value is 2GB. |
| innodb_nvdimm_buffer_pool_instances | The number of regions that the NVDIMM cache is divided into. The default value is 1. |
| innodb_nvdimm_pc_threshold_pct      | Wakeup the NVDIMM page cleaner when this % of free pages remaining. The default value is 5. |

For example:

```bash
$ vi my.cnf
...
innodb_use_nvdimm_buffer=true
innodb_nvdimm_buffer_pool_size=2G
innodb_nvdimm_buffer_pool_instances=1
innodb_nvdimm_pc_threshold_pct=5
...
```

2. Run the MySQL server:

```bash
$ ./bin/mysqld --defaults-file=my-nvdimm.cnf
``` 
