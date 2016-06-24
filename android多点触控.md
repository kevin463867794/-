#多点触控下

###以action_DOWN按下为例子
- 第一个手指的```action=0```  ;
- 第二个手指的```action=256```;即是```256```
- 第三个手指的```action=512```; 即是```256+256```这样类推 每次加```256```

**MotionEvent.ACTION_POINTER_ID_MASK**是**多点触控**的监听 
---//int是65280
```action``` &``` MotionEvent.ACTION_POINTER_ID_MASK```两个表示 即是按屏幕 而且是多点触控
同时也进行了与(&)运算 得到三只手指按上去是 512&65280 = 512    
我试着用**65280/255=256   和 11111111*100000000=1111111100000000**
```ACTION_POINTER_ID_MASK```值为8 ```ACTION_POINTER_ID_MASK```可能只是方便看

让action & MotionEvent.ACTION_POINTER_ID_MASK>>>8
举个例子
256&65280
1111111100000000& 100000000＝0000000100000000
然后>>> 8＝0000000000000001 = 1
 

可以看出大于255部分的和MASK与运算  会得到1 而小于255的都直接为0；
这样就能很好的把几只手指的id精确表达出来

得到

三只手指按上去 512>>>8 得到数字2 即使第三根手指的id
我们在程序中可以这样写

        @Override
        public boolean onTouchEvent(MotionEvent event) {
		int count = event.getPointCount();  //手指数目
		int  moreid =0; //手指id
		if(count>1)
		{
		
		moreid = (action & MotionEvent.ACTION_POINTER_ID_MASK）>>>ACTION_POINTER_ID_MASK;
		}
		case  MotionEvent.ACTION_DOWN:
		if(moreid == 0){
		     即为第一值手指在移动时候执行
		}
		if(moreid == 1){
		     即为第二值手指在移动时候执行
		}
		if(moreid == 2){
		     即为第三值手指在移动时候执行
		}break;
		}
