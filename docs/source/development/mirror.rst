Mirror Modify
=================================

Conda Mirror
-------------

The conda tool can modify its original mirrors by modify the :file:`~/.condrc`, for example:

.. code-block:: bash

    show_channel_urls: true
    default_channels:
    - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
    - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
    - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2

Pip Mirror 
-----------

The pip tool can modify its original mirrors by add a :file:`~/.pip/pip.conf`, for example:

.. code-block:: bash
    
    [global]
    index-url = https://mirrors.aliyun.com/pypi/simple