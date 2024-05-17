---
title: C++에서 엑셀파일 처리하기
date: 2018-11-30T15:14:26+09:00
categories:
  - C/C++
tags:
  - C
  - codeproject
  - Excel
  - ExcelFormat
  - xlnt
  - 라이브러리github
  - 엑셀
---
C++에서 엑셀파일을 다루는 법을 찾아보았다.

2012년도에 엑셀 파일을 처리할 방법을 찾다가 <https://dongbumkim.com/2012/05/12/handling-excel-file-on-mfc> 이런 글을 썼었는데 이제는 그보다 더 나은 방법을 찾을 수 있었다.

예전에 작업하던 소스에서는 ODBC를 이용하여 연결했었는데 왠일인지 이 방법으로는 제대로 연결되지 않았다. 당시에는 잘 작동하던 소스였는데...

찾아보니 윈도우 버전이 올라가며 제대로 작동하지 않는듯하다.(확실히 끝까지 찾아보지는 못했다.) ADO 등의 다른 방법으로 연결하는 것을 안내하고 있었다.

웹서핑을 하며 엑셀 파일을 다룰 수 있는 라이브러리를 찾아보다가 스택오버플로우에서 xlnt 라는 라이브러리를 추천해줘서 한번 테스트해보았다.

<https://github.com/tfussell/xlnt>

무려 크로스플랫폼의 C++14에 맞춘 xlsx 파일 처리 가능한 라이브러리라고 설명이 되어있다.

처음에는 잘 작동하는듯했으나... 오 잘되네 하고 본격적인 테스트코드를 만들다보니 버그가 있었다. 시트를 하나 더 추가해서 2개의 시트를 만들고 컬럼을 추가하면 에러가 떴다. 엑셀 파일을 아무리 봐도 문제가 없어 검색해봤더니 이미 버그리포팅이 되어있던 이슈. 나도 가서 한마디 거들었다.

<https://github.com/tfussell/xlnt/issues/330#issuecomment-440589168>

xlnt로 하루 넘는 시간을 날리고 다시 검색해보니 ExcelFormat 라이브러리를 추천했다. 이것으로 작업을 성공적으로 했다는 댓글도 있어서 이걸로 결정.

<https://www.codeproject.com/Articles/42504/ExcelFormat-Library>

사용해보니 다른 외부 컴포넌트 없이 바로 사용 가능했다.

단점은,

  * xlsx는 지원하지 않고 xls 파일만 지원한다.
  * 데이터를 가져올 각 셀마다 데이터타입을 정확하게 지정해줘야한다.
  * 암호 걸린 엑셀파일은 처리 불가능하다.
  * 그런데 숫자만 들어있는 셀은 전부다 double 형으로 데이터를 가져온다. int로 캐스팅해야함.
  ```cpp
    for (int k = 0; k < sheet->GetTotalRows(); ++k)
    {
      for (int j = 0; j < sheet->GetTotalCols(); ++j)
      {
          ExcelFormat::BasicExcelCell* cell = sheet->Cell(k, j);
          switch (cell->Type())
          {
          case ExcelFormat::BasicExcelCell::INT:
              std::cout << "INT:" << cell->GetInteger() << std::endl;
              break;
          case ExcelFormat::BasicExcelCell::DOUBLE:
              std::cout << "DOUBLE:" << static_cast<int>(cell->GetDouble()) << std::endl;
              break;
          case ExcelFormat::BasicExcelCell::STRING:
              std::cout << "STRING:" << cell->GetString() << std::endl;
              break;
          case ExcelFormat::BasicExcelCell::WSTRING:
              std::cout << "WSTRING:" << cell->GetWString() << std::endl;
              break;
          case ExcelFormat::BasicExcelCell::UNDEFINED:
          case ExcelFormat::BasicExcelCell::FORMULA:
          default:
              break;
          }
      }
  }
```

테스트 코드를 열심히 작성해봤더니 시트가 여러개일때도, 데이터가 꽤 많을 때에도 데이터 처리가 무난했다.

사용했던 테스트 코드의 일부.

```cpp
class DataTableMgr
{
private:
	ExcelFormat::BasicExcel xls;
};

bool DataTableMgr::LoadWeaponBombTable(void)
{
    ExcelFormat::BasicExcelWorksheet* sheet = nullptr;

    for (int i = 0; i < xls.GetTotalWorkSheets(); ++i)
    {
        sheet = xls.GetWorksheet(i);
        if (nullptr == sheet)
            continue;

        if (!strcmp(WEAPONBOM_SHEETNAME, sheet->GetAnsiSheetName()))
        {
            sheet = xls.GetWorksheet(i);
            break;
        }
    }

    if (nullptr == sheet)
        return false;

    // 첫행은 컬럼 인덱스이므로 항상 패쓰
    for (int i = 1; i < sheet->GetTotalRows(); ++i)
    {
        DATATABLE_WEAPONBOMB weapon_bomb;
        weapon_bomb.index = static_cast<int>(sheet->Cell(i, 0)->GetDouble());
        weapon_bomb.id = static_cast<int>(sheet->Cell(i, 1)->GetDouble());
        weapon_bomb.id_5pack = static_cast<int>(sheet->Cell(i, 2)->GetDouble());
        weapon_bomb.id_11pack = static_cast<int>(sheet->Cell(i, 3)->GetDouble());
        weapon_bomb.itemid = static_cast<int>(sheet->Cell(i, 4)->GetDouble());

        weapon_bomb_map.insert(DATATABLE_WEAPONBOMB_MAP::value_type(weapon_bomb.index, weapon_bomb));
    }

    std::cout << "WeaponBomb count : " << weapon_bomb_map.size() << std::endl;

    return true;
}
```

코드 자체는 잘 작동했지만, static 데이터를 엑셀 파일로 관리하는 것에 대해 위험함이 제기 되어 이 프로젝트는 무산되었다. ㅎㅎㅎ 나중에 언젠가는 쓸 일이 있겠지.
