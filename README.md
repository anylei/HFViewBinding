# HFViewBinding

[![CI Status](http://img.shields.io/travis/Hai Feng Kao/HFViewBinding.svg?style=flat)](https://travis-ci.org/Hai Feng Kao/HFViewBinding)
[![Version](https://img.shields.io/cocoapods/v/HFViewBinding.svg?style=flat)](http://cocoapods.org/pods/HFViewBinding)
[![License](https://img.shields.io/cocoapods/l/HFViewBinding.svg?style=flat)](http://cocoapods.org/pods/HFViewBinding)
[![Platform](https://img.shields.io/cocoapods/p/HFViewBinding.svg?style=flat)](http://cocoapods.org/pods/HFViewBinding)


UITableView and UICollectionView Binding 

This project privides some helper functions to bind a mutable array to table views or colllection views. It supports single or multiple sections.

## Installation

HFViewBinding is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod "HFViewBinding"
```

## Usage
```objc
KVOMutableArray* someData = [[KVOMutableArray alloc] 
                        initWithMutableArray:[@[@"cell 1", @"cell 2"] mutableCopy]];

HFTableViewBinding* bindingHelper = 
[HFTableViewBinding bindingForTableView:self.tableView 
                                   sourceList:someData 
                            didSelectionBlock:^(id model) 
                                             { NSLog(@"clicked on %@", model); } 
                        templateCellClassName:@"YourCellClass"
                                     isNested:NO];

bindingHelper.delegate = self;
bindingHelper.dataSource = self;
```
You have to use `KVOMutableArray` as the mutable array class to inform the observers about the mutation event. See [KVOMutableArray](https://github.com/haifengkao/KVOMutableArray) for more details.

The cell must implement `HFBindingDelegate` protocol.

The `delegate` and `dataSource` settings are optional. If they are set, all delegate methods which are not handled by bindingHelper will be sent to `self` in the above exmaple.

To support multiple sections, `isNested` should be set to `YES`. The each item in the array must be the class of `KVOMutableArray`.

```objc
KVOMutableArray* firstRow = [[KVOMutableArray alloc] 
                        initWithMutableArray:[@[@"cell 1", @"cell 2"] mutableCopy]];
KVOMutableArray* secondRow = [[KVOMutableArray alloc] 
                        initWithMutableArray:[@[@"cell 1", @"cell 2"] mutableCopy]];
KVOMutableArray* someData = [[KVOMutableArray alloc] initWithMutableArray:[@[firstRow, secondRow] mutableCopy]];

HFTableViewBinding* bindingHelper = 
[HFTableViewBinding bindingForTableView:self.tableView 
                                   sourceList:someData 
                            didSelectionBlock:^(id model) 
                                             { NSLog(@"clicked on %@", model); } 
                        templateCellClassName:@"YourCellClass"
                                     isNested:YES];
```
## Motivation

MVVM (Model-View-ViewModel) is a popular replacement of original MVC architecture.
It moves the UI logics from hard-to-test Controller to testable ViewModels. MVVM relies heavily on view bindings. But the available binding libraries ([HRTableCollectionBindingHelper](https://github.com/Rannie/HRTableCollectionBindingHelper), [CETableViewBinding](https://github.com/ColinEberhardt/CETableViewBinding)) don't support table view with multiple sections. Besides, the table view animations of insertion and deletion are disabled in their implmentation, which really annoys me :(

## Credit
The APIs are referenced from [HRTableCollectionBindingHelper](https://github.com/Rannie/HRTableCollectionBindingHelper).

The idea of delegate forwarding comes from [CETableViewBinding](https://github.com/ColinEberhardt/CETableViewBinding).

Big thanks to Rannie and Colin.

## Requirements

Requires iOS 7.0, and ARC.

## Author

Hai Feng Kao, haifeng@cocoaspice.in

## Contributing

Forks, patches and other feedback are welcome.

## License

HFViewBinding is available under the MIT license. See the LICENSE file for more info.
