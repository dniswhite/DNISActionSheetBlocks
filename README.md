DNISActionSheetBlocks
=====================
A very simple implementation of `UIActionSheet` that allows you to use blocks for notification instead of delegates. 

##History
This came to be because of some demonstration code for [DNISSliderDemo](https://github.com/dniswhite/sliderDemo) where I needed access to an object (`DNISItemCell`) later in the chain of dealing with a `UIActionSheet`. 

After some frustration and later acceptance of the fact that a `UITableView` will only give me access to a `UITableViewCell` via a reusable element call (not acceptable) I realized that my solution needed clojure (closure as well depending on how you look at the problem). 

##Example
In case you don't want to view the example source code from [DNISSliderDemo](https://github.com/dniswhite/sliderDemo) here is a quick snipet of the code from it.

```objc
// include the header
#import "DNISActionSheetBlocks.h"
```

Here it is being used.

```objc
-(void) moreInfoButton:(UIButton *) sender
{
    NSLog(@"more button clicked for row %d", sender.tag);
    
    // this should be the table view cell
    DNISItemCell * itemCell = (DNISItemCell *)[[[sender superview] superview] superview];
    
    // if I use a block here I can then capture the cell in the callback for the actionsheet
    DNISActionSheetBlocks *actionSheet = [[DNISActionSheetBlocks alloc] initWithButtons: @"Cancel" destructiveButtonTitle:nil otherButtonTitles:@"#1 Action Button", @"#2 Action Button", nil];
    
    [actionSheet setBlockDidDismissWithButton:^(UIActionSheet * sheet, NSInteger index) {
        NSIndexPath * indexPath = [[self tableView] indexPathForCell:itemCell];
        DNISItem * item = [[self listItems] objectAtIndex:indexPath.row];
        
        [[self tableView] setScrollEnabled:YES];
        [[self tableView] reloadRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationRight];
        
        [item setSliderOpen:NO];
        
        NSLog(@"itemCell logging %@", [[itemCell titleLabel] text]);
    }];
    
    [actionSheet setTag:[sender tag]];
    [actionSheet setActionSheetStyle:UIActionSheetStyleDefault];
    [actionSheet showInView: [self view]];
}
```

##More Information
I had originally started this with just one simple `init` method that did what I wanted but figured I would add some of the other `init` methods as general helpers.

If you like it great.