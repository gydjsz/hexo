---
title: java题库导出
---

题库链接:
jd-gui反编译工具链接:

该题库软件支持jdk1.8版本

1. 下载完成之后，打开cmd命令行窗口，在该软件目录下的jar/bin/中执行:
./java.exe -jar jd-gui-1.4.0.jar，即可打开反编译的软件

2. 将Tiku.jar文件导入eclipse中，将如下代码添加到eclipse文本窗口中

```
import java.io.BufferedWriter;
import java.io.EOFException;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileWriter;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.Serializable;
import java.util.ArrayList;

import quesion.data.Problem;

/**
 * @author Administrator
 *
 */
public class GetData implements Serializable{
    String readPath;
    String writePath;
    String chapter[];
    String type[];
    ArrayList<Problem> problems;
    File readFile;
    File writeFile;
    FileInputStream fis;
    ObjectInputStream ois;
    FileWriter fw;
    BufferedWriter bw;

    public GetData() {
        readPath = "C:\\Users\\Administrator\\Desktop\\java\\题库\\";
        writePath = "C:\\Users\\Administrator\\Desktop\\data\\";
        chapter = new String[15];
        for(int i = 0; i < 15; i++)
            chapter[i] = "第" + (i + 1) + "章\\";
        type = new String[4];
        type[0] = "读程题";
        type[1] = "判断题";
        type[2] = "挑错题";
        type[3] = "选择题";
    }

    public void importFile() {
        File directory;
        for(int i = 0; i < 15; i++) {
            for(int j = 0; j < 4; j++) {
                readFile = new File(readPath + chapter[i] + type[j] + ".data");
                directory = new File(writePath + chapter[i]);
                if(!directory.exists())
                    directory.mkdir();
                writeFile = new File(writePath + chapter[i] + type[j] + ".txt");
                if(readFile.exists()) {
                    try {
                        fis = new FileInputStream(readFile);
                        ois = new ObjectInputStream(fis);
                        fw = new FileWriter(writeFile);
                        bw = new BufferedWriter(fw);
                        problems = new ArrayList<Problem>();
                        problems = (ArrayList<Problem>)(ois.readObject());
                        for(Problem p : problems) {
                            String s = p.content;
                            int flag = 0;
                            if(p.content != null) {
                                for(int m = 0; m < s.length(); m++)
                                    if(s.charAt(m) == '\n' && flag != 0) {
                                        bw.newLine();
                                    }
                                    else {
                                        bw.write(s.charAt(m));
                                        flag = 1;
                                    }
                            }
                            bw.newLine();
                            s = p.correctAnswer;
                            flag = 0;
                            if(p.correctAnswer != null) {
                                for(int m = 0; m < s.length(); m++)
                                    if(s.charAt(m) == '\n' && flag != 0) {
                                        bw.newLine();
                                    }
                                    else {
                                        bw.write(s.charAt(m));
                                        flag = 1;
                                    }
                            }
                            bw.newLine();
                        }
                        ois.close();
                        fis.close();
                        bw.close();
                    } catch (FileNotFoundException e) {
                    } catch (IOException e) {
                    } catch (ClassNotFoundException e) {
                    }
                }

            }
        }
    }

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        GetData data = new GetData();
        data.importFile();

    }
}
```
