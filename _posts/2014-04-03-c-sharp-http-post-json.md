---
title: 'C#에서 HTTP POST로 JSON 데이터 보내는 방법'
date: 2014-04-03T13:50:44+09:00
tags:
  - C
  - JSON
  - Post
  - Request
---
회사 업무 중 HTTP POST로 Request Body에 JSON을 넣어서 보내야 할 일이 있어서 간단하게 짜본 WinForm 프로그램이다.

이런 코드들을 간단하면서도 막상 필요할 때 찾아서 쓰기가 귀찮아서 찾아보기 쉽게 여기에 적어둔다.

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Net;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApplication1
{
  public partial class Form1 : Form
  {
    public Form1()
    {
      InitializeComponent();
    }

    private void button1_Click(object sender, EventArgs e)
    {
      var httpWebRequest = (HttpWebRequest)WebRequest.Create("http://127.0.0.1:40080/Default.aspx?cmd=2");
      httpWebRequest.ContentType = "text/json";
      httpWebRequest.Method = "POST";

      using (var streamWriter = new StreamWriter(httpWebRequest.GetRequestStream()))
      {
        string json = "{"kakao_id":"1","image_url":"http://teste11111.com","public_profile":"Y"}";

        streamWriter.Write(json);
        streamWriter.Flush();
        streamWriter.Close();

        var httpResponse = (HttpWebResponse)httpWebRequest.GetResponse();
        using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
        {
          var result = streamReader.ReadToEnd();
          MessageBox.Show(result);
        }
      }
    }
  }
}
```
