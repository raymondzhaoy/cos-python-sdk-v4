Qcloud COSv4 SDK
#######################

 
.. image:: https://travis-ci.org/tencentyun/cos-python-sdk-v4.svg?branch=master
    :target: https://travis-ci.org/tencentyun/cos-python-sdk-v4

.. image:: https://badge.fury.io/py/qcloud_cos_v4.png
    :target: https://badge.fury.io/py/qcloud_cos_v4
    
.. image:: https://readthedocs.org/projects/cossdkv4/badge/?version=latest
    :target: http://cossdkv4.readthedocs.io/en/latest/?badge=latest
    
介绍
_______

`腾讯云COSv4 <https://www.qcloud.com/product/cos.html>`_ 的Python SDK, 目前可以支持Python2.6与Python2.7。

安装指南
__________

使用pip安装 ::

    pip install -U qcloud_cos_v4


手动安装::

    python setup.py install

使用方法
__________

使用python sdk，参照sample.py

.. code:: python

    # 设置用户属性, 包括appid, secret_id和secret_key
    # 这些属性可以在cos控制台获取(https://console.qcloud.com/cos)
    appid = 100000                  # 替换为用户的appid
    secret_id = u'xxxxxxxx'         # 替换为用户的secret_id
    secret_key = u'xxxxxxx'         # 替换为用户的secret_key
   region_info = "sh" #           # 替换为用户的region，目前可以为 sh/gz/tj/sgp，分别对应于上海，广州，天津,新加坡园区
    cos_client = CosClient(appid, secret_id, secret_key, region=region_info)

    # 设置要操作的bucket
    bucket = u'mybucket'

    ############################################################################
    # 文件操作                                                                 #
    ############################################################################
    # 1. 上传文件(默认不覆盖)
    #    将本地的local_file_1.txt上传到bucket的根分区下,并命名为sample_file.txt
    #    默认不覆盖, 如果cos上文件存在,则会返回错误
    request = UploadFileRequest(bucket, u'/sample_file.txt', u'local_file_1.txt')
    upload_file_ret = cos_client.upload_file(request)
    print 'upload file ret:', repr(upload_file_ret)

    # 2. 上传文件(覆盖文件)
    # 2.1上传本地文件，将本地的local_file_2.txt上传到bucket的根分区下,覆盖已上传的sample_file.txt
    request = UploadFileRequest(bucket, u'/sample_file.txt', u'local_file_2.txt')
    request.set_insert_only(0)  # 设置允许覆盖
    upload_file_ret = cos_client.upload_file(request)
    print 'overwrite file ret:', repr(upload_file_ret)
    # 2.2从内存上传文件
    request = UploadFileFromBufferRequest(bucket, u'/sample_file.txt', data)
    request.set_insert_only(0)  # 设置允许覆盖
    upload_file_ret = cos_client.upload_file_from_buffer(request)
    print 'overwrite file ret:', repr(upload_file_ret)

    # 3. 获取文件属性
    request = StatFileRequest(bucket, u'/sample_file.txt')
    stat_file_ret = cos_client.stat_file(request)
    print 'stat file ret:', repr(stat_file_ret)

    # 4. 更新文件属性
    request = UpdateFileRequest(bucket, u'/sample_file.txt')

    request.set_biz_attr(u'this is demo file')        # 设置文件biz_attr属性,不支持中文
    request.set_authority(u'eWRPrivate')              # 设置文件的权限
    request.set_cache_control(u'cache_xxx')           # 设置Cache-Control
    request.set_content_type(u'application/text')     # 设置Content-Type
    request.set_content_disposition(u'ccccxxx.txt')   # 设置Content-Disposition
    request.set_content_language(u'english')          # 设置Content-Language
    request.set_x_cos_meta(u'x-cos-meta-xxx', u'xxx') # 设置自定义的x-cos-meta-属性
    request.set_x_cos_meta(u'x-cos-meta-yyy', u'yyy') # 设置自定义的x-cos-meta-属性

    update_file_ret = cos_client.update_file(request)
    print 'update file ret:', repr(update_file_ret)

    # 5. 更新后再次获取文件属性
    request = StatFileRequest(bucket, u'/sample_file.txt')
    stat_file_ret = cos_client.stat_file(request)
    print 'stat file ret:', repr(stat_file_ret)

    # 6. 移动文件, 将sample_file.txt移动位sample_file_move.txt
    request = MoveFileRequest(bucket, u'/sample_file.txt', u'/sample_file_move.txt')
    stat_file_ret = cos_client.move_file(request)
    print 'move file ret:', repr(stat_file_ret)

    # 7. 删除文件
    request = DelFileRequest(bucket, u'/sample_file_move.txt')
    del_ret = cos_client.del_file(request)
    print 'del file ret:', repr(del_ret)

    # 8. 下载文件
    request = DownloadFileRequest(bucket, u'/sample_file_move.txt')
    del_ret = cos_client.download_file(request)
    print 'del file ret:', repr(del_ret)

    # 9. 下载文件到内存
    request = DownloadObjectRequest(bucket, u'/sample_file_move.txt')
    fp = cos_client.download_object(request)
    fp.read()

    ############################################################################
    # 目录操作                                                                 #
    ############################################################################
    # 1. 生成目录, 目录名为sample_folder
    request = CreateFolderRequest(bucket, u'/sample_folder/')
    create_folder_ret = cos_client.create_folder(request)
    print 'create folder ret:', create_folder_ret

    # 2. 更新目录的biz_attr属性
    request = UpdateFolderRequest(bucket, u'/sample_folder/', u'this is a test folder') # biz_attr不支持中文
    update_folder_ret = cos_client.update_folder(request)
    print 'update folder ret:', repr(update_folder_ret)

    # 3. 获取目录属性
    request = StatFolderRequest(bucket, u'/sample_folder/')
    stat_folder_ret = cos_client.stat_folder(request)
    print 'stat folder ret:', repr(stat_folder_ret)

    # 4. list目录, 获取目录下的成员
    request = ListFolderRequest(bucket, u'/sample_folder/')
    list_folder_ret = cos_client.list_folder(request)
    print 'list folder ret:', repr(list_folder_ret)

    # 5. 删除目录
    request = DelFolderRequest(bucket, u'/sample_folder/')
    delete_folder_ret = cos_client.del_folder(request)
    print 'delete folder ret:', repr(delete_folder_ret)
