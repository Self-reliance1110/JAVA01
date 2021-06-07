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
# 高级文件处理(续)
## 图形图像的简介及解析
### 图形
* java.awt包
* java.2D库:Graphics2D,Line2D,Rectangle2D,Ellipse2D,Arc2D
* color,storke(线条)
### 图像
* 由像素点构成
* 格式：jpg，png，gif等等
* 颜色：RGC(RED,GREEN.BLUE)
#### 图像类
Java原生支持jpg,png,bwp,wbmp,gif
* javax.imageio.ImageIO
ImageReader,ImageWriter读写图像文件
* java.awt.image.BufferedImage

## 条形码及二维码的简介及解析
### 条形码(一维码)
* 通常代表一段数字/字母
* 一般数据容量为30个数字/字母
<!--代码用例-->
```
public static void main(String[] args) {

        generateCode(new File("D:/1dCode.png"),"123456789012",500,250);
        readCode("D:/1dCode.png");
    }
    public static void generateCode(File file,String code,int weight,int height)//生成二维码
    {
        BitMatrix bitMatrix = null;//位图矩阵
        try {
            MultiFormatWriter mfw = new MultiFormatWriter();

            bitMatrix = mfw.encode(code,BarcodeFormat.CODE_128,weight,height,null);
        } catch (WriterException e) {
            e.printStackTrace();
        }

        try(FileOutputStream fop = new FileOutputStream(file))
        {
            ImageIO.write(MatrixToImageWriter.toBufferedImage(bitMatrix),"png",fop);
            fop.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public static void readCode(String file)
    {
        try {
            BufferedImage bufferedImage = ImageIO.read(new File(file));
            if(bufferedImage == null)
                return;
            LuminanceSource source = new BufferedImageLuminanceSource(bufferedImage);
            BinaryBitmap bitmap = new BinaryBitmap(new HybridBinarizer(source));
            Map<DecodeHintType,Object> hints = new HashMap<>();
            hints.put(DecodeHintType.CHARACTER_SET,"GBK");
            hints.put(DecodeHintType.PURE_BARCODE,Boolean.TRUE);
            hints.put(DecodeHintType.TRY_HARDER,Boolean.TRUE);

            Result result = new MultiFormatReader().decode(bitmap,hints);
            System.out.println("条形码内容："+result);
        } catch (IOException | NotFoundException e) {
            e.printStackTrace();
        }
    }
```

### 二维码
* 用某种特定的几何图形按照一定规律在平面分布的黑白相间的图形记录数据符号信息
* 比条形码能存储更多信息
* 能存储数字/字母/文字/图片等信息
* 可存储几百到几十kb
* 抗损坏

#### Zxing(Zebra Crossing)
Java需要依赖第三方库来对条形码或二维码进行操作
主要类
* BitMatrix位图矩阵
* MultiFormatWriter位图编写器
* MatrixToImageWriter写入图片

#### BarCode4J
* 第三方类库
* 纯Java实现的条形码生成
* 只负责生成，不负责解析

## DOCX文档功能和处理
* DOCX解析
* DOCX生成(完全生成，部分生成+模板)
### Apache POI
* 可处理docx,xlsx,pptx,visio等office套件
* 纯java工具包
## 表格文件处理xls/xlsx （Excel）
* xlsx 是以XML为标准
* 第三方类库
* POI JXL (免费)
* COM4J (Windows平台)
* Aspose等 (收费)

## PDF简介及解析
* 便携式文档格式
* PostScript 描述所有图形 保证在不同的机器上的颜色和打印效果
* 字型嵌入系统 可使字型随文件一起传输
* 结构化的存储系统 绑定元素和任何相关内容到单个文件 带有适当的数据压缩系统


# 作业
```
    private static ArrayList<Person> personList, resultPersonList;
    private static Person person;
    private static File barCodeFile;
    private static XWPFDocument document;

    public static void main(String[] args) throws IOException, WriterException, InvalidFormatException {
        resultPersonList = readExcel("student.xlsx");
        for (Person p : resultPersonList) {
            System.out.println(p);
        }

        barCodeFile = new File("barCode.png");
        generateBarCode(barCodeFile, person.getCode(), 500, 250);
        modify();
        ConvertDocxToPdf();
    }
    public static void writeBarCode(String imagePath) throws IOException, InvalidFormatException {
        XWPFParagraph xwpfParagraph = document.createParagraph();
        XWPFRun run = xwpfParagraph.createRun();
        run.addPicture(new FileInputStream(imagePath), XWPFDocument.PICTURE_TYPE_PNG, imagePath, Units.toEMU(200), Units.toEMU(40));
        FileOutputStream fileOutputStream = new FileOutputStream("student1.docx");
        document.write(fileOutputStream);
    }

    public static void modify() throws IOException, InvalidFormatException {
        InputStream is = new FileInputStream("student.docx");
        document = new XWPFDocument(is);
        List<IBodyElement> iBodyElements = document.getBodyElements();

        for (int i = iBodyElements.size()-1; i >=0 ; i--) {
            BodyElementType bodyElementType = iBodyElements.get(i).getElementType();
            if (bodyElementType == BodyElementType.TABLE) {
                XWPFTable table = (XWPFTable) iBodyElements.get(i);
                List<XWPFTableRow> tableRows = table.getRows();
                for (XWPFTableRow row : tableRows) {
                    List<XWPFTableCell> cells = row.getTableCells();
                    int studentInfoCnt = 0;
                    for (int k = 0; k < cells.size(); k++) {
                        if (cells.get(k).getText().equals("{name}")) {
                            //remove the text in the cell which already existed
                            cells.get(k).removeParagraph(0);
                            cells.get(k).setText(resultPersonList.get(studentInfoCnt).getName());
                        } else if (cells.get(k).getText().equals("{sex}")) {
                            //already removed the name cell, so the index of this cell would be previous index
                            cells.get(k).removeParagraph(0);
                            cells.get(k).setText(resultPersonList.get(studentInfoCnt++).getGender());
                        }
                    }
                }
            }
            if (bodyElementType != BodyElementType.TABLE) {
                XWPFParagraph xwpfParagraph = (XWPFParagraph) iBodyElements.get(i);
                if (xwpfParagraph.getText().equals("{barcode}")) {
                    List<XWPFRun> runs = xwpfParagraph.getRuns();
                    for (int j = runs.size()-1; j >=0; j--) {
                        xwpfParagraph.removeRun(j);
                    }
                    writeBarCode("barCode.png");
                }
            }
        }


        FileOutputStream fileOutputStream=new FileOutputStream("student1.docx");
        document.write(fileOutputStream);
        fileOutputStream.close();
    }    


    public static ArrayList<Person> readExcel(String filePath) throws IOException {
        personList = new ArrayList<>();
        InputStream ExcelFileToRead = new FileInputStream(filePath);
        XSSFWorkbook wb = new XSSFWorkbook(ExcelFileToRead);

        XSSFSheet sheet = wb.getSheetAt(0);
        XSSFRow row;
        XSSFCell cell;

        Iterator rows = sheet.rowIterator();
        while (rows.hasNext()) {
            row = (XSSFRow) rows.next();
            Iterator cells = row.cellIterator();
            person = new Person();
            int position = 0;
            while (cells.hasNext()) {
                cell = (XSSFCell) cells.next();
                switch (position) {
                    case 0:
                        person.setName(cell.getStringCellValue());
                        break;
                    case 1:
                        person.setGender(cell.getStringCellValue());
                        break;
                    case 2:
                        person.setCode(cell.getStringCellValue());
                        break;
                    default:
                }
                position++;
            }
            personList.add(person);
        }
        return personList;
    }
    public static void ConvertDocxToPdf(){
        String docxPath = "student1.docx";
        String pdfPath = "student.pdf";
        XWPFDocument document = null;
        try (InputStream doc = Files.newInputStream(Paths.get(docxPath))) {
            document = new XWPFDocument(doc);
        } catch (IOException e) {
            e.printStackTrace();
        }
        fr.opensagres.poi.xwpf.converter.pdf.PdfOptions options = PdfOptions.create();
        try (OutputStream out = Files.newOutputStream(Paths.get(pdfPath))) {
            PdfConverter.getInstance().convert(document, out, options);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void generateBarCode(File file, String code, int width, int height) throws WriterException, FileNotFoundException {
        BitMatrix matrix = null;
        MultiFormatWriter writer = new MultiFormatWriter();
        matrix = writer.encode(code, BarcodeFormat.CODE_128, width, height, null);
        FileOutputStream fileOutputStream = new FileOutputStream(file);
        try {
            ImageIO.write(MatrixToImageWriter.toBufferedImage(matrix), "png", fileOutputStream);
            fileOutputStream.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

