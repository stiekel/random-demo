title: Source code
date: 2018-04-13 18:06:53
tags:
 - code
---

js code:

```javascript
/**
 * set time
 * @param {number} h hour
 * @param {number} m minite
 * @param {number} s second
 * @param {number} ms millisecond
 */
export default (t, h, m, s, ms) => {
  if (!isNaN(h)) t.setHours(h)
  if (!isNaN(m)) t.setMinutes(m)
  // one lime comments
  if (!isNaN(s)) t.setSeconds(s)
  if (!isNaN(ms)) t.setMilliseconds(ms)
  return t
}

```

java

```java
/**
 * 删除HDFS集群中的所有空文件和空目录
 * 
 * @throws Exception
 */
public static void deleteEmptyDir(Path path) throws Exception {
  HDFSUtils.initFileSystem();
  // 当前路径就是空目录时
  FileStatus[] listFile = HDFSUtils.fs.listStatus(path);
  if (listFile.length == 0) {
    //删除空目录
    HDFSUtils.fs.delete(path, true);
    return;
  }
  //如果不是空文件，先获取指定目录下的文件和子目录
  RemoteIterator<LocatedFileStatus> listLocatedStatus = HDFSUtils.fs.listLocatedStatus(path);
  while (listLocatedStatus.hasNext()) {
    LocatedFileStatus next = listLocatedStatus.next();
           //获取当前目录和其父目录
    Path currentPath = next.getPath();
    Path parentPath=next.getPath().getParent();
    
    // 如果是文件夹，继续往下遍历
    if (next.isDirectory()) {
      // 如果是空目录，删除
      if (HDFSUtils.fs.listStatus(currentPath).length == 0) {
        HDFSUtils.fs.delete(currentPath, true);
        if(HDFSUtils.fs.listStatus(parentPath).length==0){
          HDFSUtils.fs.delete(parentPath, true);
        }
      } else {
        // 不是空目录，那么则重新遍历
        if (HDFSUtils.fs.exists(currentPath)) {
          AchieveClass.deleteEmptyDir(currentPath);
        }
      }
    // 如果是文件
    } else {
      // 获取文件的长度
      long fileLength = next.getLen();
      // 当文件是空文件时， 删除
      if (fileLength == 0) {
        HDFSUtils.fs.delete(currentPath, true);
      }
    }
    // 当空文件夹或者空文件删除时，有可能导致父文件夹为空文件夹，这里需要判断一下
    int length = HDFSUtils.fs.listStatus(parentPath).length;
    if(length == 0){
      HDFSUtils.fs.delete(parentPath, true);
    }
  }
      HDFSUtils.closeFileSystem();
}
```
