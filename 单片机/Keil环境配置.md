1. 建立项目文件夹，在内部建立两个文件夹，一个User文件夹，一个Driver文件夹。
2. Keil的project创建在User文件夹内。
3. 配置魔法棒（Configure Target Options）。
   1. 选择Output，选中Create HEX File，format选择默认。
   2. 选择C51，Include Paths将Driver文件夹引进来，作为驱动文件夹。
4. 配置品字形（Manage Project Items）。
   1. 修改Projects Targets的名称。
   2. 将User文件夹和Driver文件夹添加到Groups内。
5. 然后写底层文件，保存在Driver文件夹内。