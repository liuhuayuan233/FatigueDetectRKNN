###### 1.

前端就去修改templates里的文件，user_data_sample.html和user_video_sample.html应该没用吧

###### 2.

app.py是一个主函数，image_add_tag（）函数需要小改动，更改需要在图片上显示的信息

gen_from_stream（）函数需要主要修改，配后修改后的模型，呈现出推理的结果

###### 3.

refer_image.py是另一个需要主要修改的文件，在里面更改新的模型。trans_model（）实现模型转换，在config.py里提供模型路径即可。主要修改refer()函数，配合gen_from_stream()函数提供所需的实现。refer_utils.py就根据refer_image.py修改吧，一些功能应该能原封不动直接用

sample_data.py不太懂，是采集数据时才需要使用吗，还是定义了一个组织结构之后人脸信息都需要用到

还有datasets.py，是否需要我们更改其中的数据结构