# Tripcode \ Python3

Никто не пишет имиджборды на питоне, и такого удобного хеша просто не найти, пришлось реинжинирить алгоритм и писать самому.

### Алгоритм

Надо просто привести фразу к многобайтной кодировке, зашифровать и взять несколько символов. Очень просто?

1. надо привести фразу к многобайтной _национальной_ кодировке. для японских строк нужна кодировка `shift-jis`, для польских не знаю, но пример из вики `Zwykły tripkod: User !ozOtJW9BFA Bezpieczny tripkod: User !!Oo43raDvH61` не работает в `utf-8`
2. не просто зашифровать, а `crypt(3)` шифрованием, но библиотека `crypt` реализована только для \*nix-систем, на Windows не работает. на самом деле, достаточно алгоритма [DES](https://ru.wikipedia.org/wiki/DES)
3. последние десять символов с конца зашифрованной со специальной солью многобайтной строки

Другие варианты возможны, но не будут совместимыми с оригинальным трипкодом

<details>
  <summary>
    некоторые реализации
  </summary>

* на перле из немецкой вики https://de.wikipedia.org/wiki/Tripcode
```
$salt = substr($tripkey.'H.', 1, 2);        # tripkey ist Shift-JIS kodiert
$salt =~ s/[^\.-z]/\./go;                   # ersetze alle Zeichen kleiner als "." und größer als "z" durch "."
$salt =~ tr/:;<=>?@[\\]^_`/A-Ga-f/;         # ersetze alle Zeichen aus ":;<=>?@[\]^_`" durch ihr Äquivalent aus "ABCDEFGabcdef"
$trip = crypt($tripkey, $salt);             # Unix-crypt(3)-Funktion
$trip = substr($trip, -10);                 # entferne Salt am Anfang
print '◆'.$trip;
```

* на втором питоне https://pypi.org/project/tripcode/ с собственной реализацией шифрования. репозиторий больше не существует, проект изменился https://github.com/Cairnarvon/triptools
* парсинг имиджборд одним файлом https://py4chan.sourceforge.net/
* исходник данной реализации взят с японской текстоборды https://blog.utgw.net/entry/2021/01/05/195013
* дополнение к предыдущему: расширенная реализация для строк больше 12 символов https://github.com/utgwkk/20210103-sketch-tripcode/blob/master/tripcode.py
  
</details>


### Использование
  
```
pip install -U tripcode3
```

```
from tripcode import tripcode
print( tripcode('tea') )      # WokonZwxw2
print( tripcode(u'ｋａｍｉ') )    # yGAhoNiShI
```


### TODO
* дополнить кодировками

### License

BSDLv2
