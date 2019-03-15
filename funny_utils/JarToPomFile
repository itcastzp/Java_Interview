package com.adonai.test.demo0101;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileWriter;
import java.io.IOException;
import java.util.jar.JarInputStream;
import java.util.jar.Manifest;

import org.dom4j.Element;
import org.dom4j.dom.DOMElement;
import org.jsoup.Jsoup;

import com.alibaba.fastjson.JSONObject;

/**
 * Hello world!
 *
 */
public class App 
{
    public static void main( String[] args ) throws FileNotFoundException, IOException
    {
        //System.out.println( "Hello World!" );

        Element dependencys = new DOMElement("dependencys");
        File dir = new File("C:\\my\\software\\eclipse64\\work1\\crm01\\WebContent\\WEB-INF\\lib");  
        //需生成pom.xml 文件的 lib路径


        FileWriter fw=new FileWriter("c://my//pom.txt");
         int count=0;


        for (File jar : dir.listFiles()) {
             JarInputStream jis = new JarInputStream(new FileInputStream(jar));
             Manifest mainmanifest = jis.getManifest();
             jis.close();
            String bundleName = mainmanifest.getMainAttributes().getValue("Bundle-Name");
            String bundleVersion = mainmanifest.getMainAttributes().getValue("Bundle-Version");
            Element ele = null;
            System.out.println("jar names:=="+jar.getName());

            StringBuffer sb = new StringBuffer(jar.getName());


            if (bundleName != null) {
                  bundleName = bundleName.toLowerCase().replace(" ", "-");

                  ele = getDependices(bundleName, bundleVersion);

                  fw.write(ele.asXML());
                  fw.write(System.getProperty("line.separator"));
                  System.out.println("count:="+(++count));

                  System.out.println(ele.asXML());
            }
            if (ele == null || ele.elements().size() == 0) {
                 bundleName = "";
                 bundleVersion = "";
                 String[] ns = jar.getName().replace(".jar", "").split("-");
                 for (String s : ns) {
                      if (Character.isDigit(s.charAt(0))) {
                            bundleVersion += s + "-";
                      } else {
                            bundleName += s + "-";
                      }
                 }
                 if (bundleVersion.endsWith("-")) {
                          bundleVersion = bundleVersion.substring(0, bundleVersion.length() - 1);
                 }
                 if (bundleName.endsWith("-")) {
                          bundleName = bundleName.substring(0, bundleName.length() - 1);
                 }
                 ele = getDependices(bundleName, bundleVersion);
                 sb.setLength(0);

                 fw.write(ele.asXML());
                 fw.write(System.getProperty("line.separator"));

                 System.out.println("count:="+(++count));
                 System.out.println(ele.asXML());
         }

         ele = getDependices(bundleName, bundleVersion);
         if (ele.elements().size() == 0) {
               ele.add(new DOMElement("groupId").addText("not find"));
               ele.add(new DOMElement("artifactId").addText(bundleName));
               ele.add(new DOMElement("version").addText(bundleVersion));
         }
         dependencys.add(ele);
         System.out.println();
       }
       System.out.println("final:="+dependencys.asXML());
       fw.flush();
       fw.close();
      // fw.write(dependencys.asXML().toString());



    }



    public static Element getDependices(String key, String ver) {
          Element dependency = new DOMElement("dependency");
          // 设置代理
//         System.setProperty("http.proxyHost", "127.0.0.1");
//         System.setProperty("http.proxyPort", "8090");
          try {
           String url = "http://search.maven.org/solrsearch/select?q=a%3A%22" + key + "%22%20AND%20v%3A%22" + ver + "%22&rows=3&wt=json";
           org.jsoup.nodes.Document doc = Jsoup.connect(url).ignoreContentType(true).timeout(30000).get();
           String elem = doc.body().text();
           JSONObject response = JSONObject.parseObject(elem).getJSONObject("response");
           if (response.containsKey("docs") && response.getJSONArray("docs").size() > 0) {
            JSONObject docJson = response.getJSONArray("docs").getJSONObject(0);
            Element groupId = new DOMElement("groupId");
            Element artifactId = new DOMElement("artifactId");
            Element version = new DOMElement("version");
            groupId.addText(docJson.getString("g"));
            artifactId.addText(docJson.getString("a"));
            version.addText(docJson.getString("v"));
            dependency.add(groupId);
            dependency.add(artifactId);
            dependency.add(version);
           }
          } catch (Exception e) {
           e.printStackTrace();
          }
          return dependency;
         }

}
