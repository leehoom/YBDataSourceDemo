# YBDataSourceDemo

### 工具的主要目的是为了解决controller页面代码耦合度高问题。

+ 解决了vc里tableView的delegate和DataSource臃肿的问题；
+ 定义基类YBTableViewController，封装tableView以及它的代理方法；
+ 通过继承实现复用与解耦，使用简单方便；

### 使用方法

新建UIViewController继承自`YBTableViewController`,然后就可以通过以下三种方式调用：

+ 调用系统的UITableViewCell


		__weak __typeof(&*self)weakSelf = self;
		    
		self.dataArray = @[@[@1,@2,@3],@[@1,@2]];
		    
	    self.tableViewDS.cellWillDisplayAtIndexPath = ^(UITableViewCell *cell, NSIndexPath *indexPath, id item) {
	        cell.textLabel.text = @"name";
	    };
	    self.tableViewDS.numberOfSectionsInTableView = ^NSInteger(NSArray *tableData) {
	        return tableData.count;
	    };
	    self.tableViewDS.didSelectRowAtIndexPath = ^(NSIndexPath *indexPath, id item) {
	        YBListVC *listVC = [[YBListVC alloc]init];
	        [weakSelf.navigationController pushViewController:listVC animated:YES];
	    };
	    
	    
+ 实现自定义的tableViewCell

		[self initDatasourceWithCellClass:[YBTestCell class]];
	    self.tableViewDS.cellWillDisplayAtIndexPath = ^(UITableViewCell *cell, NSIndexPath *indexPath, id item) {
	        YBTestCell *myCell = (YBTestCell *)cell;
	        myCell.selectionStyle = UITableViewCellSelectionStyleNone;
	        [myCell.iconImageView setImage:[UIImage imageNamed:@""]];
	        myCell.contentLB.text = @"王颖博";
	    };
	    self.tableViewDS.numberOfSectionsInTableView = ^NSInteger(NSArray *tableData) {
	        return tableData.count;
	    };
	    
+ 通过重写父类方法`- (void)initDatasource`调用

		- (void)initDatasource
		{
		    [self.tableView registerClass:[YBTestCell class] forCellReuseIdentifier:@"testCell"];
		    
		    __weak __typeof(&*self)weakSelf = self;
		    self.tableViewDS = [[YBDataSource alloc] initWithTableData:self.dataArray cellIdentifier:@"testCell" cellForRowAtIndexPath:^(id cell, NSIndexPath *indexPath, id item) {
		        
		        YBTestCell *myCell = (YBTestCell *)cell;
		        myCell.selectionStyle = UITableViewCellSelectionStyleNone;
		        [myCell.iconImageView setImage:[UIImage imageNamed:@""]];
		        myCell.contentLB.text = @"王颖博";
		    }];
		    
		    self.tableViewDS.numberOfSectionsInTableView = ^NSInteger(NSArray *tableData) {
		        return tableData.count;
		    };
		    
		    self.tableViewDS.didSelectRowAtIndexPath = ^(NSIndexPath *indexPath, id item) {
		        YBTestVC *testVC = [[YBTestVC alloc]init];
		        [weakSelf.navigationController pushViewController:testVC animated:YES];
		    };
		    
		    [self.tableView setDataSource:self.tableViewDS];
		    [self.tableView setDelegate:self.tableViewDS];
		}
		
### end