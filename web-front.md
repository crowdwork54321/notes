1. Text-align:center; line-height=行高； 可以让行级元素及行内块元素相对于容器水平居中和垂直居中。（注意css要放在容器上）； 

2. Vertical-align: center 可以让文字和图片中线对齐。 Vertical-align直接加在元素本身上。 

3. 去除图片底部的空白间隙：

   img {

   ​       Vertical-align:top

   }

4. flex部局：display:flex;

5. flex为弹性布局， 任何一个元素都可以使用flex布局， 当为父元素设置为flex布局之后，则float,clear ,vertical-align则失效。

6. 固定定位的元素是脱离文档流的，要单独设置宽度(以下是wap中指定内容居中对齐），固定定位的盒子，margin:0 auto无法让容器居中。 

   ~~~css
   .search{
     position:fixed;
     top:0;
     left:50%;
     transform:translateX(-50%);
     width:100%;
     max-width:540px;
     min-width:320px;
     height:44px;
     background-color:pink
   }
   ~~~

7. Before  ，after 伪类元素的使用：

   ~~~css
    .mine::before {
               content: "";
               display: block;
               width: 23px;
               height: 23px;
               margin: 3px auto;
               background: url(./img/close.png) no-repeat;
               background-size: 23px 23px;
           }
   ~~~

   



