逻辑表达式包含了“&&”、“||”与“！”三种常见的逻辑表达式，其组合通常可以构成更复杂的使用。

#### &&

左表达式为假值直接返回左表达式。

    var objA = {};
    var objB = {x:0};
    var objC = {y:1};
    var res = objA && objB; // {}
    var res = objA && objC; // {}

左表达式为真值时返回右表达式的值。

    var objA = {x:0};
    var objB = {y:1};
    var objC = {z:1};
    var res = objA && objB; // {y:1}
    var res = objA && objC; // {z:1}


#### ||

左表达式为真值直接返回左表达式。

    var objA = {x:0};
    var objB = {};
    var objC = {};
    var res = objA && objB; // {x:0}
    var res = objA && objC; // {x:0}

左表达式为真值时返回右表达式的值。

    var objA = {};
    var objB = {x:0};
    var objC = {y:1};
    var res = objA && objB; // {x:0}
    var res = objA && objC; // {y:1}

常用于在一组数值中选择出第一个真值表达式。

    var max = max_width || prefer_max || 500;

    funcion copy(o,p){
        p = p || {};
        //
    }

#### !

作用是取反,总是返回true/false,可以通过两次“!!”获取与原值的等价布尔值。

    var flagA = true;
    var flagB = false;
    ## 简单的例子
    var res = !flagA; // false;
    var res = !flagB; // true;
    ## 下式永远成立。
    !(p && q) = !p || !q;
    !(p || q) = !p && !q;

