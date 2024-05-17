---
title: MFC에서 데이터를 엑셀 파일로 저장하는 방법
date: 2012-05-12T22:15:18+09:00
categories:
  - C/C++
tags:
  - Excel
  - MFC
  - 엑셀
  - 저장
---

**2018년 11월 30일 추가.**

아래의 내용보다 더 좋은 엑셀파일 처리 라이브러리를 찾았다.

<https://dongbumkim.com/2018/11/30/handling-excel-in-cplusplus>

---

MFC에서 자료를 엑셀파일로 저장하는 방법을 찾아보았다.

여러가지 방법들이 많이 있었는데 그중에 가장 괜찮은 방법이어서 나중에 잊어버리지 않도록 여기에 써둔다.

아래 내용을 이용하기 위한 파일은 [XLAutomation](/assets/attach/XLAutomation.zip) 이다.

혹시나 이 글을 볼 사람들이 있을까봐 아래의 코드는 [짬식(wingyui)님 블로그](http://blog.naver.com/wingyui?Redirect=Log&logNo=30070208599)의 내용이며 이 코드 및 내용에 대한 모든 권리는 짬식(wingyui)님에게 있다는 것을 밝혀둔다.

```cpp
int i=1,j=1,k=1;
 int columnNum=0;
 char temp[10];
 CString m_strFileName;

 CXLEzAutomation XL(FALSE); // FALSE: 처리 과정을 화면에 보이지 않는다

 m_strFileName = "Student Quiz Results";   // 저장할 파일 Name

 // column 데이터
 XL.SetCellValue(++columnNum, 1, "Login ID");
 XL.SetCellValue(++columnNum, 1, "Student ID");
 XL.SetCellValue(++columnNum, 1, "Name");

 //column 에 Quiz 번호 만큼 추가
 for(i=1;i<=m_iQuizNum;i++){
  _snprintf(temp, 10, "Quiz %d", i);
  XL.SetCellValue(++columnNum, 1 ,temp);
 }

 //학생수와 Quiz 수만큼 리스트 컨트롤에서 엑셀로 가져옴.
 for(k=1; k<=m_iStudentNum; k++)
  for(j=1; j<=columnNum; j++)
   XL.SetCellValue(j, k+1, m_ctrlScroeList.GetItemText(k-1, j));

 CFileDialog cFDlg(false,"xlsx",m_strFileName+".xlsx", OFN_HIDEREADONLY |
  OFN_OVERWRITEPROMPT | OFN_NOCHANGEDIR, "xlsx 파일 (*.xlsx)|*.xlsx|", NULL );

 if(cFDlg.DoModal() == IDOK)
  XL.SaveFileAs(cFDlg.GetPathName());

 XL.ReleaseExcel();
```

코드는 그다지 어렵지 않다. 하나 중요한 점은 Column과 Row 번호를 지정할 때는 0부터 시작하는 것이 아니라 1부터 시작한다는 것. 프로그래밍 할때 모든 루프는 다 0부터 시작하다보니 어색하게 느껴진다. 반드시 1부터 넣을 것.

또 하나 중요한 점은 파일을 저장할 때 확장명을 xlsx로 해야 잘 열린다. xls로 해도 저장이 되긴 하는데 MS Excel 2010으로 열려고 하니 형식이 이상하다는 경고가 한번씩 떴다. xlsx로 저장하니 오류 없이 잘 열린다.
