# 鸿蒙Next开发教程：一行代码实现数据存储
在鸿蒙开发中，比较常用的数据存储方式是用户首选项Preferences。

但是用户首选项使用起来比较麻烦，拿存储数据来说，第一步获取Preferences实例，第二步将数据存入本地，第三步调用flush方法实现数据持久化。

这一套下来的代码大概是这样：

```
try {
    dataPreferences.getPreferences(getContext(this),'DB_NAME',(err, preferences) => {
      preferences.put('name','bob',(err)=>{
        if (err) {
          console.error(`Failed to put data. Code:${err.code}, message:${err.message}`);
          return;
        }
        preferences.flush()
        console.info('Succeeded in putting data.');
      })
    })
}catch (err){
}
```
如果我们每次存取数据都要写这么一坨代码难免让人崩溃，所以要把它封装一下，在程序中实现一行代码就完成数据的存取。

因为每次使用用户首选项都要获取实例，所以先写一个获取实例的方法：

```
try {
    dataPreferences.getPreferences(getContext(this),'DB_NAME',(err, preferences) => {
      preferences.put('name','bob',(err)=>{
        if (err) {
          console.error(`Failed to put data. Code:${err.code}, message:${err.message}`);
          return;
        }
        preferences.flush()
        console.info('Succeeded in putting data.');
      })
    })
}catch (err){
}
```
然后封装一个存储数据方法：

```
async putData( key: string,data: string,db_name: string = defaultPreferenceName) {
    if (!preference) {
      await this.getPreferencesFromStorage(db_name);
    }
    try {
        await preference.put(key, data);
    } catch (err) {
    }
    await preference.flush();
  }
```
读取数据方法：

```
async getData( key: string,db_name: string = defaultPreferenceName) {
    if (!preference) {
      await this.getPreferencesFromStorage(db_name);
    }
    return await preference.get(key, '')
  }
```
然后当在程序中需要存储数据时：

PreferenceModel.putData('name','bob')
读取数据时：

let name = await PreferenceModel.getData('name')
这样就完成了数据的存取，非常方便。

完整的封装代码如下：

```
import dataPreferences from '@ohos.data.preferences';

let context = getContext(this);
let preference: dataPreferences.Preferences;
let preferenceTemp: dataPreferences.Preferences;
const defaultPreferenceName: string = "DB_NAME"

class PreferenceModel {
  async getPreferencesFromStorage(db_name: string) {
    try {
      preference = await dataPreferences.getPreferences(context, db_name);
    } catch (err) {
    }
  }

  /**
   * 删除
   */
  async deleteLocalPreferences(key) {
    try {
      await dataPreferences.deletePreferences(context,key);
    } catch(err) {
    };
    preference = preferenceTemp;
  }

  /**
   * 存储
   */
  async putData( key: string,data: string,db_name: string = defaultPreferenceName) {
    if (!preference) {
      await this.getPreferencesFromStorage(db_name);
    }
    try {
        await preference.put(key, data);
    } catch (err) {
    }
    await preference.flush();
  }

  /**
   * 取数据
   */
  async getData( key: string,db_name: string = defaultPreferenceName) {
    if (!preference) {
      await this.getPreferencesFromStorage(db_name);
    }
    return await preference.get(key, '')
  }
}

export default new PreferenceModel();
```
