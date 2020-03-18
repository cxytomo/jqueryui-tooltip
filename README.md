# jqueryui-tooltip
based on jqueryui toolitp, fix the bug of arrow position
#目的
修复当整个tooltip距离右边为负时，即放不下时，整个tooltip需要左移到刚好能放下为止，此时toolttip下面的小尾巴会定位有问题。
原生插件只提供了定位在左边20%和中间位置的定位，这里进行了修改使得右边放不下时小尾巴定位到target的地方。
css样式很神奇，定位超出页面界限可能引起元素的宽度发生变化。
#如何使用
```
// js
$('.jq-tooltip').tooltip({
        position: {
          my: 'center bottom-20',
          at: 'center top',
          using: function( position, feedback ) {
            $( this ).css( position );
            $( '<div>' )
              .addClass( 'tooltip-arrow' )
              .addClass( feedback.vertical )
              .appendTo( this );
              var left = feedback.target.left - feedback.element.left;
              var right =  left + feedback.target.width - feedback.element.width;
              var extrClass = '';
              if (right > 0) {
                  extrClass += (left > 0 ? 'right' : 'center');
                  $(this).find('.tooltip-arrow').addClass(extrClass);
              } else {
                  $(this).find('.tooltip-arrow').css({
                      left: 'auto', //覆盖原生的left定位值
                      right: feedback.element.width - left - 35 - 3 - feedback.target.width/2 + 'px' // 35为小尾巴一半的宽度
                  });
              }
          }
        }
    });
```
