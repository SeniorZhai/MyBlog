title: 表格(UITableView)和表格控制器(UITableViewController)
date: 2014-05-23 12:37:25
categories: iOS
tags: [iOS]
---
- UITableView实质是一个列表（单列表格）
    + UITable继承了UIScrollView，主要封装了UITabelViewCell单元格控件。所以，UITabelView默认是可以对单元格进行滚动，默认情况下，所有的UITabelViewController实例被自动设为UIScrollView委托。
    + 设置属性
        1. Style
            + Plain:指定该表格使用最普通的风格。
            + Grouped:指定该表格使用分组风格。
        2. Separator
            + 分割条样式：可选择Single Line(单线)和Single Line Etched(被蚀刻的单线)
            + 分隔条颜色
        3. Selection
            + No Selection:不允许选中
            + Single Selection:只允许单选
            + 允许多选
        4. Editing
            + No Selection During Editing:编辑状态时不允许选中
            + Single Selection During Editing:编辑状态时只允许单选
            + Multiple Selection During Editing:编辑状态时允许多选
    + 属性和方法
        - style:只读属性，用于返回该表格的样式，可以返回UITabelViewStylePlain(普通)和UITabelViewStyleGrounped(分组)两个样式。
        - rowHeight:该属性用于返回或设置表格的行高，通常来说，建议实现表格对应的委托对象的tabelView:heightForRowAtIndexPath:方法来设置行高。
        - separatorStyle:该属性用于返回或设置表格的分割线样式。它支持UITabelViewCell SeparatorStyleNone(无分隔线)、UITabelViewCellSeparatorStyleSingleLine(单线分隔条)和UITableViewCellSeparatorStyleSingleLineEtched(被蚀刻的单线分隔条)这三个枚举值。
        - separatorColor:该属性用于设置分隔条的颜色。
        - backgroundView:该属性用于返回或设置表格的背景控件，它可以设置一个任意的UIView控件，该控件自动缩放到匹配该表格。
        - tabelHeaderView:该属性可设置或返回该表格的页眉控件。
        - tabelFooterView:该属性可设置或返回该表格的页脚控件。
        - numberOfRowsInSection:该方法返回指定分区包含的行数。
        - numberOfSections:该方法返回表格所包含的分区数。
    + 设置数据UITableViewDataSource对象
        - tabelView:cellForRowAtIndexPath:必需方法，该方法返回的UITabelViewCell对象，将作为指定IndexPath对应表格行的控件。
        - tabelView:numberOfRowsInSection:必需方法，该方法返回的NSInteger值，决定指定分区包含的表格行数量。
        - numberOfSectionInTableView:可选方法，该方法返回的NSInteger值决定该表格所包含的分区数量。如果不实现该方法，该表格默认只包含一个分区。
- UITableViewCell三个可配置的属性
    + textLabel:该属性是一个UILabel控件，代表UITableViewCell显示的标题。
    + detailTextLabel:该属性是一个UILabel控件，代表该UITableViewCell显示的详细内容。
    + image:该属性是一个UIImageView对象，代表UITabelViewCell坐标的图标。
- UITableViewCell4种不同的风格
    + UITableViewCellStyleSubtitle:detailTextLabel字体略小，显示在textLabel的下方。
    + UITableViewCellStyleDefault:detailTextLabel，只显示textLabel。
    + UITableViewCellStyleValue1：deltailTextLabel以淡蓝色显示，显示在表格的右边。
    + UITableViewCellStyleValue2：deltailTextLabel以淡蓝色、小字体显示，detailTextLabel以大字体显示在表格右边，不显示image控件。
- UITableView访问表格控件的表格行和分区的方法
    + -cellForRowAtIndexPath:返回该表格中指定NSIndexPath对应的表格栏
    + -indexPathForCell:获取该表格中指定表格行对应的NSIndexPath
    + -indexPathForRowAtPoint:返回该表格中指定点所在的NSIndexPath
    + -indexPathForRowInRect:返回该表格中指定区域内所有NSIndexPath组成的数组
    + -visibleCells:返回该表格中所有可见区域内的表格行组成的数组
    + -indexPathsForVisibleRows:返回该表格中多有可见区域内的表格行对应的NSIndexPath组成的数组
- UITableView控制表格控件滚动的方法
    + scrollToRowAtIndexPath:atScrollPosition:animated:控件该表格滚动到指定的NSIndexPath对应的表格行的顶端、中间或下方。
    + -scrollToNearestSelectedRowAtScrollPosition:animated:控件该表格滚动到选中表格行的顶端、中间或下方。
- UITableView配置表格被选中状态的属性
    + allowsSelection:该属性控制表格是否被运行选中
    + allowsMultipleSelection:该属性控制表格是否被允许多选
    + allowsSelectionDuringEditing:该属性控制该表格出于编辑状态时是否允许被选中。
    + allowsMultipleSelectionDuringEdting:该属性控制控制该表格处于编辑状态时是否允许多选。
- UITableview操作表格被选中的方法
    + -indexPathForSelectedRow:获取选中表格行对应的NSIndexPath
    + -indexPathsForSelecedRows:获取所有被选中表格行对应的NSIndexPath组成的数组
    + -selectRowAtIndexPath:animated:scrollPosition:控制该表格选中指定的NSIndexPath对应的表格行，最后一个参数控制是否滚动到被选中行的顶端、中间和底部
    + -deselectRowAtIndexPath:animated:取消选中该表格中指定NSIndexPath对应的表格行。
- 要响应表格行的选中事件，需要委托实现UITableViewDelegate，其中定义了如下方法：
    + -tableView:willSelectRowAtIndexPath:当用户将要选中表格中某行时激发的方法
    + -tableView:didSelectRowAtIndexPath:当用户完成选中某行时激发的方法
    + -tableView:willDeselectRowAtIndexPath:当用户将要取消选中某行激发的方法
    + -tableView:didDeselectRowAtIndexPath:当用户取消选中某行时激发的方法
