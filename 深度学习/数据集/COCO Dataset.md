# COCO Dataset

coco数据集可视化工具（还有其他很多功能可以研究一下）

https://voxel51.com/docs/fiftyone/

## COCO object detection annotation format

### annotation总结构

```json
{
    "info": info, 数据集基础属性
    "images": [image], 图片信息
    "annotations": [annotation], 标注信息
    "categories": [category], 标签信息
    "licenses": [license], 开源许可信息
}
```

### info

```json
{
    "year": int,
    "version": str,
    "description": str, 
    "contributor": str,
    "url": str,
    "date_created": datetime, 
}
```

### images

```json
{
    "id": int, 组内id
    "width": int, 图片宽
    "height": int, 图片高
    "file_name": str, 图片名称
    "license": int, 开源许可索引
    "flickr_url": str, 图片网络路径（farm7.staticflickr下的位置）
    "coco_url": str, 图片网络路径（images.cocodataset下的位置）
    "date_captured": datetime, 图片获取时间
}
```

### categories

```json
{
    "id": int,
    "name": str,
    "supercategory": str,
}
```

### annotation

```json
{
    "id": int, 组内id
    "image_id": int, 图片id，对应images中的id
    "category_id": int, 类别id，对应categories中的id
    "segmentation": RLE or [polygon], 分割多点信息（其中polygon为一维list，[x, y, x, y, ..., x, y]的格式）
    "area": float, 分割面积大小
    "bbox": [x_left_up,y_left_up,width,height], 矩形框信息
    "iscrowd": 0 or 1, 是否包含多个目标（一般我们都是用0，一般在目标很密集，一个框包含多个目标使用1，并且segmentation也会用RLE）
}
```

### license

```json
{
    "id": int,
    "name": str,
    "url": str,
}
```