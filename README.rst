This module provides a function to calculate tripcodes:

    $ pip install -U tripcode3
    >>> from tripcode import tripcode
    >>> tripcode('tea')
    'WokonZwxw2'
    >>> tripcode(u'ｋａｍｉ')
    'yGAhoNiShI'

It don't use ``crypt(3)`` implementation, but require crossplatform ``passlib``

Inspired  by https://pypi.org/project/tripcode/
