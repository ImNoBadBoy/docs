.. _cn_api_vision_datasets_VOC2012:

VOC2012
-------------------------------

.. py:class:: paddle.vision.datasets.VOC2012()


    `VOC2012 <http://host.robots.ox.ac.uk/pascal/VOC/voc2012/>`_ 数据集。

参数
:::::::::
        - data_file (str) - 数据集文件路径，如果 ``download`` 参数设置为 ``True`` ， ``data_file`` 参数可以设置为 ``None`` 。默认值为 ``None`` 。
        - mode (str) - ``'train'`` 或 ``'test'`` 模式，默认为 ``'train'`` 。
        - download (bool) - 当 ``data_file`` 是 ``None`` 时，该参数决定是否自动下载数据集文件。默认为 ``True`` 。
        - backend (str，optional) - 指定要返回的图像类型：PIL.Image或numpy.ndarray。必须是{'pil'，'cv2'}中的值。如果未设置此选项，将从paddle.vsion.get_image_backend获得这个值。 默认值： ``None`` 。

返回
:::::::::

VOC2012数据集实例。

代码示例
:::::::::

        .. code-block:: python

            import paddle
            from paddle.vision.datasets import VOC2012
            from paddle.vision.transforms import Normalize

            class SimpleNet(paddle.nn.Layer):
                def __init__(self):
                    super(SimpleNet, self).__init__()

                def forward(self, image, label):
                    return paddle.sum(image), label

            normalize = Normalize(mean=[0.5, 0.5, 0.5],
                                  std=[0.5, 0.5, 0.5],
                                  data_format='HWC')
            voc2012 = VOC2012(mode='train', transform=normalize, backend='cv2')

            for i in range(10):
                image, label= voc2012[i]
                image = paddle.cast(paddle.to_tensor(image), 'float32')
                label = paddle.to_tensor(label)

                model = SimpleNet()
                image, label= model(image, label)
                print(image.numpy().shape, label.numpy().shape)

    
