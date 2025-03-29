# 如何在鸿蒙Next开发中使用数据库
鸿蒙中的数据库基于SQLite组件，用来处理关系比较复杂的数据，本文将以WORKER表为例，为大家演示在鸿蒙开发中对数据库的增删改查操作。

1、首先导入数据库模块：
```
import relationalStore from '@ohos.data.relationalStore';

```
2、配置数据库信息：

```
const STORE_CONFIG :relationalStore.StoreConfig= {

      name: 'RdbTest.db', // 数据库文件名

      securityLevel: relationalStore.SecurityLevel.S1, // 数据库安全级别

      encrypt: false, // 可选参数，指定数据库是否加密，默认不加密

      dataGroupId: 'dataGroupID' // 可选参数，仅可在Stage模型下使用，表示为应用组ID，需要向应用市场获取。指定在此Id对应的沙箱路径下创建实例，当此参数不填时，默认在本应用沙箱目录下创建。

    };
```
3、获取RdbStore实例，要注意的是，此处的getContext(this)用于获取应用上下文:

```
const STORE_CONFIG :relationalStore.StoreConfig= {

      name: 'RdbTest.db', // 数据库文件名

      securityLevel: relationalStore.SecurityLevel.S1, // 数据库安全级别

      encrypt: false, // 可选参数，指定数据库是否加密，默认不加密

      // dataGroupId: 'dataGroupID' // 可选参数，仅可在Stage模型下使用，表示为应用组ID，需要向应用市场获取。指定在此Id对应的沙箱路径下创建实例，当此参数不填时，默认在本应用沙箱目录下创建。

    };



    const SQL_CREATE_TABLE = 'CREATE TABLE IF NOT EXISTS WORKER (ID INTEGER PRIMARY KEY AUTOINCREMENT, NAME TEXT NOT NULL, AGE INTEGER, GENDER TEXT NOT NULL)'; 



    relationalStore.getRdbStore(getContext(this),STORE_CONFIG,(err,store)=>{

      if (err) {

        console.error(`Failed to get RdbStore. Code:${err.code}, message:${err.message}`);

        return;

      }

      console.info('Succeeded in getting RdbStore.');



      // 当数据库创建时，数据库默认版本为0

      if (store.version === 0) {

        store.executeSql(SQL_CREATE_TABLE); // 创建数据表

        // 设置数据库的版本，入参为大于0的整数

        store.version = 3;

      }



   



    })
```


插入数据

插入的数据类型为ValuesBucket，需要先引入ValuesBucket模块：

```
import { ValuesBucket } from '@ohos.data.ValuesBucket';



const valueBucket1: ValuesBucket = {

        'NAME': 'Bob',

        'AGE': '18',

        'GENDER': '男',

      };

if (store !== undefined) {

   (store as relationalStore.RdbStore).insert('WORKER', valueBucket1, (err: BusinessError, rowId: number) => {

       if (err) {

            console.error(`Failed to insert data. Code:${err.code}, message:${err.message}`);

            return;

          }

          console.info(`Succeeded in inserting data. rowId:${rowId}`);

     })

}
```
修改数据

以修改Bob的年龄为例：

```
const valueBucket2: ValuesBucket = {

        'NAME': 'Bob',

        'AGE': '24',

        'GENDER': '男',

};

      

// 修改数据

let predicates = new relationalStore.RdbPredicates('WORKER');

predicates.equalTo('NAME', 'Bob'); 

if (store !== undefined) {

   (store as relationalStore.RdbStore).update(valueBucket2, predicates, (err: BusinessError, rows: number) => {

       if (err) {

          console.error(`Failed to update data. Code:${err.code}, message:${err.message}`);

          return;

        }

          console.info(`Succeeded in updating data. row count: ${rows}`);

     }

}
```
删除数据：

```
// 删除数据
predicates.equalTo('NAME', 'Bob');

  if (store !== undefined) {

     (store as relationalStore.RdbStore).delete(predicates, (err: BusinessError, rows: number) => {

        if (err) {

          console.error(`Failed to delete data. Code:${err.code}, message:${err.message}`);

          return;

        }

        console.info(`Delete rows: ${rows}`);

   }

}
```


查询数据：

```
if (store !== undefined) {

        (store as relationalStore.RdbStore).query(predicates, ['ID', 'NAME', 'AGE', 'GENDER'], (err: BusinessError, resultSet) => {

          if (err) {

            console.error(`Failed to query data. Code:${err.code}, message:${err.message}`);

            return;

          }

          console.info(`ResultSet column names: ${resultSet.columnNames}, column count: ${resultSet.columnCount}`);

          // resultSet是一个数据集合的游标，默认指向第-1个记录，有效的数据从0开始。

          while (resultSet.goToNextRow()) {

            const id = resultSet.getLong(resultSet.getColumnIndex('ID'));

            const name = resultSet.getString(resultSet.getColumnIndex('NAME'));

            const age = resultSet.getLong(resultSet.getColumnIndex('AGE'));

            const gender = resultSet.getDouble(resultSet.getColumnIndex('GENDER'));

            console.info(`id=${id}, name=${name}, age=${age}, gender=${gender}`);

          }

          // 释放数据集的内存

          resultSet.close();

        })

      }
```
