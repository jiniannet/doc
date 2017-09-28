---
layout: jntemplate-zh-CN
---

# JNTemplate Syntax
JNTemplate���﷨�ɷ�Ϊ������ǩ���д��ǩ��������ǩ��ʽΪ��${tagName} ��д��ʽΪ��$tagName�������������ʽ����ʹ��������ǩ�⣬�����󲿱�ǩ�����Լ�д��
��ģ����������ж�������������������������ѭ���¹��򣺼�ֻ��ʹ����ĸ�������롰_������ϣ��ұ�������ĸ��ͷ��
���ĵ������ڰ汾1.3���ϣ�1.2�汾������ͬ��

## ����:

>${title} �� $title - �򵥱���. ���������title����ֵ��

ʾ��
```
var templateContent = "hello,${title}";
var template = (Template)Engine.CreateTemplate(templateContent);
template.Set("title", "JNTemplate");
var result = template.Render();
//�������� "hello,JNTemplate"
```

## ����:
>${model.Name} �� $model.Title - ��������.��ȡ����model�������ԡ���ֵ

ʾ��
```
var templateContent = "hello,${model.Name}";
var template = (Template)Engine.CreateTemplate(templateContent);
template.Set("model", new {
                Name="JNTemplate",
                Url="http://www.jiniannet.com",
                License="Apache License 2.0"
            });
var result = template.Render();
//�������� "hello,JNTemplate"
```

## �������ʽ:
>${ expressions } -  ֧�ּӼ��˳���ȡ���(+,-,*,/,%)�����

ʾ��
```
var templateContent = "(3+15)/2 = ${(3+15)/2}";
var template = (Template)Engine.CreateTemplate(templateContent);
var result = template.Render();
//�������� "(3+15)/2 = 9"
```

## ����
>${helper.query(parameter1,parameter2,...)} or $helper.query(parameter1,parameter2,...) - ���÷�����֧�ֵ��ö���ʵ����������FuncHandlerί�У��������ã�������֧�־�̬����������ʹ�øñ�ǩ������ʵ�־��󲿷ֹ��ܡ�

ʾ��1������ʵ��������

Helper�����:
```
public class Helper
{
    public string Show(string name, string url,string license)
    {        return string.Format("product name:{0},url:{1},license:{2}", name, url,license);
    }
}

```

��̨����:
```
var templateContent = "$helper.Show(name,model.Url,\"Apache License 2.0\")";
var template = (Template)Engine.CreateTemplate(templateContent);
template.Set("model", new {
                Url="http://www.jiniannet.com"
            });
template.Set("name", "JNTemplate");
var result = template.Render();

//�������� "product name:JNTemplate,url:http://www.jiniannet.com,license:Apache License 2.0" 
```

ʾ��2������DateTime��ToString��ʽ�����ڣ�

��̨����:
```
var templateContent = "$now.ToString(\"yyyy-MM-dd\")";//��ǰ����Ϊ2017/09/09
var template = (Template)Engine.CreateTemplate(templateContent);
template.Set("$now", DateTime.Now);
var result = template.Render();

//�������� "2017-09-09" 
```

ʾ��3��ί�з�����
```
var templateContent = "$input(\"parameter1\",\"parameter2\")";
var template = (Template)Engine.CreateTemplate(templateContent);
template.Set("input", new FuncHandler(args =>
{
    var sb = new  StringBuilder();
    sb.Append("your input:");
    foreach (var node in args)
    {
        sb.Append(node);
        sb.Append(" ");
    }
    return sb.ToString();
}));
var result = template.Render();

//�������� "your input:parameter1 parameter2"
```

## �����ļ�:
>${load("filename.html")} or $load("filename.html")-  ��ָ���ļ�����filename.html����������ǰģ���У����뵱ǰģ�干��ģ�������ļ����ݣ�����ļ��ڴ���JNTemplateģ���ǩ�����Զ����н������ñ�ǩ֧����Ŀ¼��·���ָ���Ϊ������windows����linuxxͳһΪ��/����

ע�⣺�ñ�ǩ����Ϊģ��������ָ����ǰ·Ŀ¼���ԡ�CurrentPath����������������ָ��ģ�幤��Ŀ¼��ResourceDirectories����������ʹ�á����ʹ��Engine.LoadTemplate������ʵ�� ������Զ�ָ����ǰĿ¼��

ʾ��
public/header.html����
```
<nav>
  <ul>
      <li><a href="${model.SiteUrl}">${model.SiteTitle}</a></li>
  </ul>
</nav>
```

index.html����
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>JNTemplate deomo</title>
  </head>
  <body>
      ${load("public/header.html")}
  </body>
</html>

```

C#����
```
var template = (Template)Engine.LoadTemplate("C:\\wwwwroot\index.html");
template.Set("model", new {
                model.SiteTitle="JNTemplate",
                SiteUrl="http://www.jiniannet.com" 
            });
var result = template.Render
```

��������
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>JNTemplate deomo</title>
  </head>
  <body>
	<nav>
	  <ul>
		  <li><a href="http://www.jiniannet.com">JNTemplate</a></li>
	  </ul>
	</nav>
  </body>
</html>

```

 
 
## �����ļ�:
>${include("filename.html")} or include("filename.html")-  �÷���ע�������ͬload����ͬ���Ǹ��ļ����������ģ���ǩ������н�����ֻ��򵥵Ľ����ݰ���������


## �߼��ж�:
>${ if(expressions1) }...code1..${elseif(expressions1)}...code2..{else}...code3..${end} -  �߼��жϣ����expressions1����������code1,���expressions2�����������code2,�������������code3.֧�������������||(�߼���);&&(�߼���);>(����);<��С�ڣ�;>=�����ڵ��ڣ�;<=��С�ڵ��ڣ�;!=�������ڣ�==(����)

ע�⣺���ʽ����������߼������ʱ���磺if(id),if(true),if(150)������ѭ�������£�������ʽΪ�������� (true/false)ֱ��ȡֵ�����Ϊ���֣�Ϊ0ʱ��ʾfalse������Ϊtrue�����Ϊ�ַ�����Ϊ�ջ���ΪnullʱΪfalse������Ϊtrue�����Ϊ��������nullΪfalse������Ϊtrue��elseif��elseû������ʱ����ʡ�ԡ��ñ�ǩ���ڴ������ñ���foreachIndex����ʼֵΪ1��ע�⣬��ʼֵΪ1��������������һ����0��ʼ���ر�ע�⣩

ʾ��
```
var templateContent = "${if((10%2)==0)} 10�ܱ�2���� ${else} 10����2������ ${end}";
var template = (Template)Engine.CreateTemplate(templateContent);
var result = template.Render();
//�������� "10�ܱ�2����"
```

## ѭ��:
>${ foreach(itemName in list) }...${end} -  foreach��ǩ����ѭ�����ʼ����Ի�ȡ������Ϣ��ʹ�÷�����Ҫ�����c#�е�foreachһ�£���������ʵ���� System.Collections.IEnumerable�ӿڵĶ��󶼿��Ա������������飬List<T>��DataTable.Rows�ȡ�itemNameΪ�´����ı������������Զ��塣��ʹ��JS���û���Ҫע��foreach in��js�е�for in�ǲ�ͬ�ģ�

ע�⣺�ñ�ǩ���ڴ������ñ���foreachIndex����ʼֵΪ1��

ʾ��
```
var templateContent = @"
<ul>
${foreach(book in books)}
	${if((foreachIndex%2)==0)}
	<li>${foreachIndex}�� ${book}</li>
	${else}
	<li style=""background-color:#ccc"">${foreachIndex}�� ${book}</li>
	${end}
${end}
</ul>
";
var template = (Template)Engine.CreateTemplate(templateContent);
template.Set("books", new string[]{
                "Tales of the City",
				"The Serial",
				"East of Eden",
				"Island of the Blue Dolphins"
            });
var result = template.Render();
```

��������
```
<ul>
	<li style="background-color:#ccc">1�� Tales of the City</li>
	<li>2�� The Serial</li>
	<li style="background-color:#ccc">3�� East of Eden</li>
	<li>4�� Island of the Blue Dolphins</li>
</ul>
```

## ��ֵ:
>${ set(varName=itenValue)} -  �ı������ֵ���������򴴽�

ע�⣺��֧��Ϊ���Ը�ֵ�������ѭ�����ڣ�foreach��ʹ�øñ�ǩ ����Ҫע����������ѭ���崴���ı������뿪ѭ���󽫻���գ��������ѭ�����ڸı�ı�����ѭ����������Ȼ��Ч����������ѭ��ǰ�Ѿ����ڣ��ı�ᱣ�棬�������ѭ���д����ģ��뿪ѭ����ʧЧ����

ʾ��
```
var templateContent = "${set(value=(3+5)))}value��ֵ��${value}";
var template = (Template)Engine.CreateTemplate(templateContent);
var result = template.Render();
//�������� "value��ֵ��8"
```
