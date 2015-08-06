Sorting in Mongoose has evolved over the releases such that some of these answers are no longer valid. As of the current 3.5.7 release of Mongoose, a descending sort on the date field can be done in any of the following ways:

使用Mongoose做nodejs应用开发的时候，需要对输出的数据进行排序如倒序正序，很多内容已经不适用了，这里是找到的可以用的一个排序使用方式。

    # 
    # Code
    + Room.find({}).sort('-date').exec(function(err, docs) { ... });
    + Room.find({}).sort({date: -1}).exec(function(err, docs) { ... });
    + Room.find({}).sort({date: 'desc'}).exec(function(err, docs) { ... });
    + Room.find({}).sort({date: 'descending'}).exec(function(err, docs) { ... });
    + Room.find({}, null, {sort: {date: -1}}, function(err, docs) { ... });
    + Room.find({}, null, {sort: [['date', -1]]}, function(err, docs) { ... });

Link:
	
[In Mongoose, how do I sort by date? (node.js)](http://stackoverflow.com/questions/5825520/in-mongoose-how-do-i-sort-by-date-node-js)