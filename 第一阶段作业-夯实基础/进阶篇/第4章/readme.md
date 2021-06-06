# 高级文件处理（第四章上）
## XML文件简介
* XML(eXtensible Markup Language)
* 可扩展标记语言：意义+数据
* 标签可自行定义，具有自我描述性
* 纯标签语言，跨平台，系统，语言

## XML结构
### XML常规语法
* 任何一个起始标签都要有一个结束标签
* 可以简化，如<name></name>可以写成<name/>，简写的前提是前后标签中间没有值
* 区分大小写
* 标签必须按照合适的顺序嵌套，不可以错位
* 标签的属性必须有值，而且值要加上双引号
* 需要转移字符
* 注释 <!--注释内容-->

### DTD(Document Type Definite)
* 定义XML的结构
* 使用一系列合法元素定义文档结构
* 可以嵌套在XML文件中，也可以在xml文件中引用

### XML Schema(XSD,XML Schema Definition)
* 定义xml结构，DTD的继任者
* 支持数据类型，可扩展，功能更强大，更完善
* 采用xml编写

### XSL
* 扩展样式表语言，xsl作用于xml，就相当于css作用于html
* 内容：
    xstl：转换xml文档
    XPath：在xml文件中导航
    xsl-fo：格式化xml文档


## XML文件的解析
### 树结构
#### DOM:Document Object Model
* 擅长读小文件/写
* 直观易用
* 将XML整个作为类似于树结构的方式读入内存以操作解析
* 解析大数据量的XML文件，可能会遇到内存泄漏和程序崩溃的风险

##### DOM类
* DocumentBuilder解析类，parse方法
* Node节点主接口，getChildNodes返回一个NodeList
* NodeList节点列表，每个元素是一个Node
* Document文档根节点
* Element标签节点元素(每一个标签都是标签节点)
* Text节点(包含在XML元素内的，都算Text节点)
* ttr节点(每个属性节点)

标签与标签之间的空白也会被上级父节点视为子元素
使用例子如下
```
public class demo01 {
    public static void main(String[] args) {
        //method1("users.xml");
        //method2("users.xml","name");
        writer();
    }

    public static void method1(String fileName) {//自上向下、读

        try {
            DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
            DocumentBuilder db = dbf.newDocumentBuilder();
            Document document = db.parse(fileName);

            NodeList userList = document.getChildNodes();
            System.out.println(userList.getLength());

            for (int i = 0; i < userList.getLength(); i++) {
                Node users = userList.item(i);

                NodeList nodeList = users.getChildNodes();
                for (int i1 = 0; i1 < nodeList.getLength(); i1++) {
                    if(nodeList.item(i1).getNodeType() == Node.ELEMENT_NODE)
                    {
                        NodeList nodeList1 = nodeList.item(i1).getChildNodes();
                        for (int i2 = 0; i2 < nodeList1.getLength(); i2++) {
                            if(nodeList1.item(i2).getNodeType() == Node.ELEMENT_NODE)
                            {
                                System.out.println(nodeList1.item(i2).getNodeName()+":"+nodeList1.item(i2).getTextContent());
                            }
                        }
                    }
                }
            }
        } catch (ParserConfigurationException e) {
            e.printStackTrace();
        } catch (SAXException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void method2(String fileName,String elementName)//根据名称进行搜索、读
    {
        try {
        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();

            DocumentBuilder db = dbf.newDocumentBuilder();
            Document document = db.parse(fileName);

            Element element = document.getDocumentElement();
            NodeList nodeList = element.getElementsByTagName(elementName);

            if(nodeList!=null)
            {
                for (int i = 0; i < nodeList.getLength(); i++) {
                    System.out.println(nodeList.item(i).getNodeName()+":"+nodeList.item(i).getTextContent());
                }
            }
        } catch (ParserConfigurationException e) {
            e.printStackTrace();
        } catch (SAXException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public  static  void writer()
    {
        try {
        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
        DocumentBuilder db = dbf.newDocumentBuilder();
        Document document = db.newDocument();

        if(document!=null)
        {
            Element docx = document.createElement("document");
            Element element = document.createElement("element");
            element.setAttribute("type","paragraph");
            element.setAttribute("alignment","left");
            Element object = document.createElement("object");
            object.setAttribute("type","text");
            Element text = document.createElement("text");
            text.setTextContent("abcdefg");
            Element bold = document.createElement("bold");
            bold.setTextContent("true");

            object.appendChild(text);
            object.appendChild(bold);
            element.appendChild(object);
            docx.appendChild(element);
            document.appendChild(docx);


            TransformerFactory transformerFactory = TransformerFactory.newInstance();
            Transformer transformer = transformerFactory.newTransformer();
            DOMSource domSource = new DOMSource(document);

            File file = new File("dom_result.xml");
            StreamResult streamResult = new StreamResult(file);

            transformer.transform(domSource,streamResult);
            System.out.println("successful");


        }

        } catch (ParserConfigurationException | TransformerConfigurationException e) {
            e.printStackTrace();
        } catch (TransformerException e) {
            e.printStackTrace();
        }
    }
}

```

### 流结构
#### SAX：Simple API for XML
* 更快，更轻量，擅长读大文件，流模型中的推模型
* 有选择的解析和访问，内存要求较低
* 解析是一次性读取，很难同时访问文档中的多处数据
* 当Sax没遇到一个节点就会引发一个事件，而我们需要编写这些事件的处理程序，关键类DefaultHandler

#### Stax：The Stream API for XML
* 擅长读大文件，流模型的拉模型
* 在遍历文档时，会把感兴趣的部分从读取器中拉出来，不需要引起事件，允许我们选择性地处理节点。这大大提高了灵活性，以及整体效率
* 两套API，基于指针，XMLStreamReader，基于迭代器，XMLEventReader

## JSON简介及解析
* JavaScript Object Notation,JS对象表示法
* 是一种轻量级的数据交换格式
* 类似于xml，更小更快，更易解析

### JSONObject
就是类似于键值对，例如"name":"Tom"
JSON对象：{"name":"Tom"，"...":"..."}
* 数据在键值对中
* 数据由逗号分隔
* 花括号保存对象

### JSONArray
用方括号来保存数组
```
[{"name":"Tom"，"...":"..."},{"name":"Tom"，"...":"..."}]
```

### Java的JSON处理
需要依赖第三方类库进行处理，以下是使用最多的
* org.json
```
public static  void testObject()//JSONObject的生成
    {
        Person p = new Person();
        p.setName("Tom");
        p.setAge(18);
        p.setSource(Arrays.asList(60,70,80));
        JSONObject obj = new JSONObject();
        obj.put("name",p.getName());
        obj.put("age",p.getAge());
        obj.put("source",p.getSource());
        System.out.println(obj);
        System.out.println("name"+obj.getString("name"));
        System.out.println("age"+obj.getInt("age"));
        System.out.println("source"+obj.getJSONArray("source"));
    }

    public static void testFile()//JSON文件的解析
    {
        File file = new File("book.json");
        try(FileReader fr = new FileReader(file))
        {
            int fileLength = (int)file.length();
            char[] chars = new char[fileLength];
            fr.read(chars);
            String s = String.valueOf(chars);
            JSONObject jsonObject = new JSONObject(s);
            JSONArray books = jsonObject.getJSONArray("books");
            List<BOOK> bookList = new ArrayList<>();

            for(Object book:books)
            {
                JSONObject jsonObject1 = (JSONObject) book;
                BOOK book1 = new BOOK();
                book1.setAuthor(((JSONObject) book).getString("author"));
                book1.setYear(((JSONObject) book).getString("year"));
                book1.setTitle(((JSONObject) book).getString("title"));
                book1.setPrice(((JSONObject) book).getInt("price"));
                book1.setCategory(((JSONObject) book).getString("category"));
                bookList.add(book1);

            }



        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```
* GSON
```
public static void testObject()//生成
    {
        Person p = new Person();
        p.setName("Tom");
        p.setAge(18);
        p.setSource(Arrays.asList(60,70,80));
        Gson gson = new Gson();

        //从Java对象转化为JSON字符串
        String s = gson.toJson(p);
        System.out.println(s);

        //从JSON字符串转化为Java对象
        Person p2 = gson.fromJson(s,Person.class);
        System.out.println(p2);
        //调用Gson的JsonObject
        JsonObject jsonObject = gson.toJsonTree(p).getAsJsonObject();
        System.out.println(jsonObject.get("name"));
        System.out.println(jsonObject.get("age"));
        System.out.println(jsonObject.get("source"));
    }

    public static void testFile()//读取
    {
        Gson gson = new Gson();
        File file = new File("books2.json");
        try(FileReader fr =new FileReader(file))
        {
            List<BOOK> books = gson.fromJson(fr,new TypeToken<List<BOOK>>(){}.getType());
            for(BOOK book:books)
            {
                System.out.println(book);
            }


        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```
* jackson

### JSON的主要用途
* JSON生成
* JSON解析
* JSON校验
* 与Java Bean进行互解析

# 作业
```
public class Test {
    public static void main(String[] args) {

        String s = readXML();//读取XML文件，并转化为JSON字符串形式
        writeXML(s);

    }

    public static void writeXML(String s)
    {
        Gson gson = new Gson();
        Student stu= gson.fromJson(s,Student.class);//将JSON字符串转化为JSON对象,然后变成java bean对象
        //System.out.println(stu);
        //调用Gson的JsonObject
        JsonObject jsonObject = gson.toJsonTree(stu).getAsJsonObject();
        

        try {
            DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
            DocumentBuilder db = dbf.newDocumentBuilder();
            Document document = db.newDocument();

            if(document!=null)
            {
                Element student = document.createElement("student");

                Element name = document.createElement("name");
                name.setTextContent(stu.getName());

                Element subject = document.createElement("subject");
                subject.setTextContent(stu.getSubject());

                Element score = document.createElement("score");
                score.setTextContent(stu.getScore());

                student.appendChild(name);
                student.appendChild(subject);
                student.appendChild(score);

                TransformerFactory transformerFactory = TransformerFactory.newInstance();
                Transformer transformer = transformerFactory.newTransformer();
                DOMSource domSource = new DOMSource(student);

                File file = new File("score2.xml");
                StreamResult streamResult = new StreamResult(file);

                transformer.transform(domSource,streamResult);
                System.out.println("successful");
            }

        } catch (ParserConfigurationException | TransformerConfigurationException e) {
            e.printStackTrace();
        } catch (TransformerException e) {
            e.printStackTrace();
        }
    }


    public static String readXML()
    {
        StringBuilder sb = new StringBuilder();
        try {
            DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
            DocumentBuilder db = null;
            db = dbf.newDocumentBuilder();
            Document document = db.parse("score.xml");
            NodeList nodes = document.getChildNodes();
            NodeList nodeList = nodes.item(0).getChildNodes();
            sb.append("{");

            for (int i = 0; i < nodeList.getLength(); i++) {
                if(nodeList.item(i).getNodeType() == Node.ELEMENT_NODE)
                {
                    sb.append("\"");
                    sb.append(nodeList.item(i).getNodeName());
                    sb.append("\"");
                    sb.append(":");
                    sb.append("\"");
                    sb.append(nodeList.item(i).getTextContent());
                    sb.append("\"");
                    if(i!=nodeList.getLength()-2)
                        sb.append(",");
                }
            }
            sb.append("}");
        } catch (ParserConfigurationException e) {
            e.printStackTrace();
        } catch (SAXException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

        return sb.toString();
    }
}

public class Student {
    private String name;
    private String subject;
    private String score;

    public Student() {
    }
}


//生成的score2.xml如下
<?xml version="1.0" encoding="UTF-8"?>
<student>
    <name>80</name>
    <subject/>
    <score/>
</student>
```
