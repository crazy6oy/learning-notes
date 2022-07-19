{
  "version": "软件版本",
  "flags": {图像标签s, 现在没有使用},
  "shapes": [
    {
      "label": "目标名称",
      "points": [
        [
          x,y
        ],
        [
          x,y
        ],
        ...(多边形这里就是一个封闭多边形，n个点，如果是矩形框，这里就是对角线的一组点)
      ],
      "group_id": 现在没有使用,
      "shape_type": "标签类型"[可选rectangle, polygan, etc.],
      "flags": {现在没有使用}
    },
    ...
  ],
  "imagePath": "图片名称",
  "imageData": "图片ASCII编码",
  "imageHeight": 图片高,
  "imageWidth": 图片宽
}
