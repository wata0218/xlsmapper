======================================
アノテーションの使用方法
======================================

本ライブラリでは、アノテーションは :doc:`マッピング用 <annotation_mapping>` 、:doc:`型変換用 <annotation_converter>` 、 :doc:`数式指定用 <annotation_formula>` 、:doc:`ライフサイクル・コールバック用 <annotation_lifecycle>` の4種類が存在します。

* マッピング用は、Excelのシート中の表とJavaクラスのプロパティとを関連づけるために使用します。
* 型変換用は、Excelのシート中のセルの値をJavaのクラスに設定する際に、変換処理が必要な場合に使用します。
* 数式指定用は、書き込み時のセルの数式を指定するために使用します。
* ライフサイクルコールバック用は、読み込みと書き込み処理の前、後それぞれの処理イベントにおいて、任意の処理を実装できます。


----------------------------------------------------
ExcelとJava間で数値をマッピングする際の注意事項
----------------------------------------------------

ExcelとJavaとでは表現可能な数値の有効桁数がなどが異なるため、桁落ちなどのため注意が必要です。

* 文字列型のセルの値をJavaの数値型にマッピングする場合、Javaで表現可能な最大値/最小値を超えてマッピングした場合、エラーとなります。
    
  * ver0.5以降の場合、``com.gh.mygreen.xlsmapper.cellconvert.TypeBindException`` が発生します。
  * ver0.4以前の場合、オーバーフローを起こして読み込まれます。
      例えば、shortに最大値+1(32768)がセルに設定されている場合、ーバーフローを起こして、``-32768`` として読み込まれます。

* Excelでは有効桁数が15桁であるため、Javaのlong型など15桁を超える表現が可能な数値を書き込んだ場合、数値が丸められます。
   
  * 例えば、long型の19桁の数値 ``1234567890123456789`` を書き込んだ場合、16桁以降の値が丸められ ``1234567890123450000`` として書き込まれます。
  * Excelの仕様については、`Excel の仕様と制限 <https://support.office.com/ja-jp/article/Excel-%E3%81%AE%E4%BB%95%E6%A7%98%E3%81%A8%E5%88%B6%E9%99%90-1672b34d-7043-467e-8e27-269d656771c3?ui=ja-JP&rs=ja-JP&ad=JP>`_ を参照してください。

--------------------------------------
アノテーションの定義位置
--------------------------------------

XlsMapperでは、フィールドとメソッドの両方にアノテーションを付与することができます。
アノテーションは、フィールドまたはフィールドに対するアクセッサメソッドに定義可能です。
読み込みと書き込みの両方を行うならば、フィールドに定義することをお薦めします。

* クラスの場合、全てのアクセスレベル（public/protected/private/なし）に付与したアノテーションが有効になります。
    
  * ただし、継承元の親クラスに定義されたものは無効です。
  * 内部クラスとして定義する場合、静的な ``static class`` で定義する必要があります。
    
* フィールドの場合、全てのアクセスレベル（public/protected/privte/なし）に付与したアノテーションが有効になります。
* メソッドの場合、publicのみ有効になります。
   
  * メソッドに付与した場合、読み込み時はsetterに付与したアノテーションを用います。
    書き込み時は、getterに付与したアノテーションを用います。
  * フィールドと対応するメソッド（getter/setter）の両方にアノテーションを付与した場合、メソッドに付与したアノテーションが優先されます。
  * 読み込み時と書き込み時の両方を行うことを考慮すると、フィールドにアノテーションを付与することをお薦めします。
   

.. note::
    
    アクセッサメソッドを動的に生成する `Lombok <http://projectlombok.org/>`_ を利用する場合は、フィールドに定義してください。


.. 以降は、埋め込んで作成する
.. include::  ./annotation_mapping.rst
.. include::  ./annotation_converter.rst
.. include::  ./annotation_formula.rst
.. include::  ./annotation_lifecycle.rst


