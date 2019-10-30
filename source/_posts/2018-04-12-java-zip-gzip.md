---
title: Java ZIP 和 GZIP
date: 2018-04-12 09:49:25
categories: Java
---

## 一、ZIP 压缩解压

JDK deflate ——这是 JDK 中的又一个算法（zip文件用的就是这一算法）。它与 gzip 的不同之处在于，你可以指定算法的压缩级别，这样你可以在压缩时间和输出文件大小上进行平衡。可选的级别有 0（不压缩），以及1(快速压缩)到9（慢速压缩）。它的实现是 
- java.util.zip.DeflaterOutputStream
- java.util.zip.InflaterInputStream

```java
import java.io.*;
import java.util.zip.*;
public class Main {
    public static void main(String[] args) {
        // 待压缩文件
        String testFilePath = "D:\\zip-test\\test.mp4";
        // 压缩后的文件
        String destFileName = "D:\\ziptest.zip";
        // 待压缩文件夹
        String testFolderPath = "D:\\zip-test";

        // 解压后的文件
        String decompressFolder = "D:\\zipTestFolder";

        try {
            // 压缩解压 文件
            ZipUtil.compress(testFilePath, destFileName);
            ZipUtil.decompress(destFileName, decompressFolder);
            // 压缩解压 文件夹
            ZipUtil.compress(testFolderPath, destFileName);
            ZipUtil.decompress(destFileName, decompressFolder);
        } catch (Exception e) {
            e.printStackTrace();//失败
        }
    }
}

/**
 * zip 压缩与解压主要依靠 java api 的两个类：
 * ZipInputStream
 * ZipOutputStream
 */
class ZipUtil {
    private static final String TAG = "ZipUtil";
    /**
     * 解压文件到指定文件夹
     * @param zip      源文件
     * @param destPath 目标文件夹路径
     * @throws Exception 解压失败
     */
    public static void decompress(String zip, String destPath) throws Exception {
        //参数检查
        if (zip.isEmpty() || destPath.isEmpty()) {
            throw new IllegalArgumentException("zip or destPath is illegal");
        }
        File zipFile = new File(zip);
        if (!zipFile.exists()) {
            throw new FileNotFoundException("zip file is not exists");
        }
        File destFolder = new File(destPath);
        if (!destFolder.exists()) {
            if (!destFolder.mkdirs()) {
                throw new FileNotFoundException("destPath mkdirs is failed");
            }
        }
        ZipInputStream zis = null;
        BufferedOutputStream bos = null;
        try {
            zis = new ZipInputStream(new BufferedInputStream(new FileInputStream(zip)));
            ZipEntry ze = null;
            while ((ze = zis.getNextEntry()) != null) {
                // 得到解压文件在当前存储的绝对路径
                String filePath = destPath + File.separator + ze.getName();
                if (ze.isDirectory()) {
                    new File(filePath).mkdirs();
                } else {
                    ByteArrayOutputStream baos = new ByteArrayOutputStream();
                    byte[] buffer = new byte[1024];
                    int count;
                    while ((count = zis.read(buffer)) != -1) {
                        baos.write(buffer, 0, count);
                    }
                    byte[] bytes = baos.toByteArray();
                    File entryFile = new File(filePath);
                    // 创建父目录
                    if (!entryFile.getParentFile().exists()) {
                        if (!entryFile.getParentFile().mkdirs()) {
                            throw new FileNotFoundException("zip entry mkdirs is failed");
                        }
                    }
                    // 写文件
                    bos = new BufferedOutputStream(new FileOutputStream(entryFile));
                    bos.write(bytes);
                    bos.flush();
                }
            }
        } finally {
            closeQuietly(zis);
            closeQuietly(bos);
        }
    }

    /**
     * @param srcPath  源文件的绝对路径，可以为 文件 或 文件夹
     * @param destPath 目标文件的绝对路径  /sdcard/.../file_name.zip
     * @throws Exception 解压失败
     */
    public static void compress(String srcPath, String destPath) throws Exception {
        // 参数检查
        if (srcPath.isEmpty() || destPath.isEmpty()) {
            throw new IllegalArgumentException("srcPath or destPath is illegal");
        }
        File srcFile = new File(srcPath);
        if (!srcFile.exists()) {
            throw new FileNotFoundException("srcPath file is not exists");
        }
        File destFile = new File(destPath);
        if (destFile.exists()) {
            if (!destFile.delete()) {
                throw new IllegalArgumentException("destFile is exist and do not delete.");
            }
        }
        CheckedOutputStream cos = null;
        ZipOutputStream zos = null;
        try {
            // 对目标文件做 CRC32 校验，确保压缩后的 zip 包含 CRC32 值
            cos = new CheckedOutputStream(new FileOutputStream(destPath), new CRC32());
            // 装饰一层 ZipOutputStream，使用 zos 写入的数据就会被压缩
            zos = new ZipOutputStream(cos);
            // 设置压缩级别 0-9,0 表示不压缩，1 表示压缩速度最快，9 表示压缩后文件最小；
            // 默认为 6，速率和空间上得到平衡。
            zos.setLevel(9);
            if (srcFile.isFile()) {
                compressFile("", srcFile, zos);
            } else if (srcFile.isDirectory()) {
                compressFolder("", srcFile, zos);
            }
        } finally {
            closeQuietly(zos);
        }
    }

    // 处理文件夹压缩
    private static void compressFolder(String prefix, File srcFolder, ZipOutputStream zos) throws IOException {
        String new_prefix = prefix + srcFolder.getName() + "/";
        File[] files = srcFolder.listFiles();
        // 支持空文件夹
        if (files.length == 0) {
            compressFile(prefix, srcFolder, zos);
        } else {
            for (File file : files) {
                if (file.isFile()) {
                    compressFile(new_prefix, file, zos);
                } else if (file.isDirectory()) {
                    compressFolder(new_prefix, file, zos);
                }
            }
        }
    }

    /**
     * 压缩 文件 和 空目录
     * @param prefix
     * @param src
     * @param zos
     * @throws IOException
     */
    private static void compressFile(String prefix, File src, ZipOutputStream zos) throws IOException {
        // 若是文件,那肯定是对单个文件压缩
        // ZipOutputStream 在写入流之前，需要设置一个 zipEntry
        // 注意这里传入参数为文件在 zip 压缩包中的路径，所以只需要传入文件名即可
        String relativePath = prefix + src.getName();
        if (src.isDirectory()) relativePath += "/";//处理空文件夹
        ZipEntry entry = new ZipEntry(relativePath);
        // 写到这个 zipEntry 中，可以理解为一个压缩文件
        zos.putNextEntry(entry);
        InputStream is = null;
        try {
            if (src.isFile()) {
                is = new FileInputStream(src);
                byte[] buffer = new byte[1024 << 3];
                int len = 0;
                while ((len = is.read(buffer)) != -1) {
                    zos.write(buffer, 0, len);
                }
            }
            //该文件写入结束
            zos.closeEntry();
        } finally {
            closeQuietly(is);
        }
    }

    private static void closeQuietly(final Closeable closeable) {
        try {
            if (closeable != null) {
                closeable.close();
            }
        } catch (final IOException ioe) {
            ioe.printStackTrace();
        }
    }
}
```

## 二、GZIP 压缩解压

这是一个压缩比高的慢速算法，压缩后的数据适合长期使用。JDK 中的 
- java.util.zip.GZIPInputStream
- java.util.zip.GZIPOutputStream

```java
import java.io.*;
import java.util.zip.*;
public class Main {
    public static void main(String[] args) {
        // 待压缩文件
        String testFilePath = "D:\\gzip-test\\test.mp4";
        // 压缩后的文件
        String destFileName = "D:\\gziptest.gzip";
        // 待压缩文件夹
        String testFolderPath = "D:\\gzip-test";

        // 解压后的文件
        String decompressFolder = "D:\\gzipTestFolder";

        try {
            // 压缩解压 文件
            GZipUtil.compress(testFilePath, destFileName);
            GZipUtil.decompress(destFileName, decompressFolder);
            // 压缩解压 文件夹
            GZipUtil.compress(testFolderPath, destFileName);
            GZipUtil.decompress(destFileName, decompressFolder);
        } catch (Exception e) {
            e.printStackTrace();//失败
        }
    }
}

/**
 * GZip 压缩与解压主要依靠 java api 的两个类：
 * GZipInputStream
 * GZipOutputStream
 */
class GZipUtil {
    /**
     * 解压文件到指定文件夹
     * @param gzip     源文件
     * @param destPath 目标文件绝对路径
     * @throws Exception 解压失败
     */
    public static void decompress(String gzip, String destPath) throws Exception {
        //参数检查
        if (gzip.isEmpty() || destPath.isEmpty()) {
            throw new IllegalArgumentException("gzip or destPath is illegal");
        }
        File gzipFile = new File(gzip);
        if (!gzipFile.exists()) {
            throw new FileNotFoundException("gzip file is not exists");
        }
        File destFile = new File(destPath);
        if (destFile.exists()) {
            if (!destFile.delete()) {
                throw new FileNotFoundException("destFile delete is failed");
            }
        }

        GZIPInputStream zis = null;
        BufferedOutputStream bos = null;
        try {
            zis = new GZIPInputStream(new BufferedInputStream(new FileInputStream(gzipFile)));
            bos = new BufferedOutputStream(new FileOutputStream(destPath));
            byte[] buffer = new byte[1024 << 3];
            int len = 0;
            while ((len = zis.read(buffer)) != -1) {
                bos.write(buffer, 0, len);
            }
            bos.flush();
        } finally {
            closeQuietly(zis);
            closeQuietly(bos);
        }
    }

    /**
     * @param srcPath
     * @param destPath
     * @throws Exception
     */
    public static void compress(String srcPath, String destPath) throws Exception {
        //参数检查
        if (srcPath.isEmpty() || destPath.isEmpty()) {
            throw new IllegalArgumentException("srcPath or destPath is illegal");
        }
        File srcFile = new File(srcPath);
        if (!srcFile.exists() || srcFile.isDirectory()) {
            throw new FileNotFoundException("srcPath file is not exists");
        }
        File destFile = new File(destPath);
        if (destFile.exists()) {
            if (!destFile.delete()) {
                throw new IllegalArgumentException("destFile is exist and do not delete.");
            }
        }
        GZIPOutputStream zos = null;
        InputStream is = null;
        try {
            zos = new GZIPOutputStream(new BufferedOutputStream(new FileOutputStream(destPath)));
            is = new FileInputStream(srcFile);
            byte[] buffer = new byte[1024 << 3];
            int len = 0;
            while ((len = is.read(buffer)) != -1) {
                zos.write(buffer, 0, len);
            }
            zos.flush();
        } finally {
            closeQuietly(is);
            closeQuietly(zos);
        }
    }
    private static void closeQuietly(final Closeable closeable) {
        try {
            if (closeable != null) {
                closeable.close();
            }
        } catch (final IOException ioe) {
            ioe.printStackTrace();
        }
    }
}
```

## 参考
- [Java 不同压缩算法的性能比较](http://www.importnew.com/14410.html)
- [Java 压缩与解压工具类](https://blog.csdn.net/y22222ly/article/details/52201675)


