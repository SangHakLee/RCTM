# Fatal error: Allowed memory size of 536870912 bytes exhausted (tried to allocate 84 bytes) in 

## Error case

```php
<?php
    $objPHPExcel = new PHPExcel();

    $objPHPExcel->setActiveSheetIndex(0);
    $objPHPExcel->getActiveSheet()->setTitle('엑셀');
    // ...

    header('Content-Type: application/vnd.ms-excel'); 
    header('Content-Disposition: attachment;filename="test.xlsx"'); 
    header('Cache-Control: max-age=0');
    header("Content-Description: PHP5 Generated Data");
    $objWriter = PHPExcel_IOFactory::createWriter($objPHPExcel, 'Excel2007');
    $objWriter->save('php://output');
```
> 보통의 경우에선 정상작동 하지만, Excel 행의 수가 1만 개가 넘어가고 각 행의 데이터가 20 칼럼 정도 될 때 정상적으로 다운할 수 없다.
> 다운로드한 파일의 크기가 `1KB`이고 파일을 제대로 열 수 없다고 나온다.

## Solution
[PHP 메모리 부족](http://zetawiki.com/wiki/PHP_%EB%A9%94%EB%AA%A8%EB%A6%AC_%EB%B6%80%EC%A1%B1)
```php
<?php
    ini_set('memory_limit','1024M'); // 엑셀 데이터가 많기 때문에 기본 512M 으로 불가
    $objPHPExcel = new PHPExcel();
    // ...
    // ...
```

