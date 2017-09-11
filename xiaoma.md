##xiaoma
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>������������</title>
  <style>
    * {
      margin: 0;
      padding: 0;
    }

    #map {
      width: 800px;
      height: 600px;
      background-color: #CCCCCC;
      position: relative;
    }
  </style>
</head>
<body>
<div id="map">

</div>


<script>
  //ʳ��Ĺ��캯����
  (function () {
    function Food(options) {
      //���캯����ʹ�ö�������ĺô������εĹ��ǱȽ��٣�����չ��ǿ
      //ʳ����Ҫ���õ����ԣ���߱���ɫ�������������꣬��ͼ
      this.width = options.width || 20;          //����ʳ����ӿ��
      this.height = options.height || 20;        //����ʳ����Ӹ߶�
      this.bgColor = options.bgColor || "green"; //����ʳ����ӱ���ɫ
      this.map = options.map;                    //�����ͼ
      //x��y���Ա���������꣨��ǰ������һ�����ӣ����Ǿ����left��topֵ��
      this.x = 0;//�ڳ�ʼ��ʳ��Ľ׶Σ��������ֵ������
      this.y = 0;
      this.element;                              //������������ʳ��Ľṹ������
    }

    Food.prototype.init = function () {
      //6 ͨ�����ܷ��������Ƿ���ÿ��һ��ʳ�ﱻ�Ե��󣬶���Ҫ���´����µ�ʳ�����֮ǰ��Ҫ��ɾ�����Ե��ľ�ʳ��
      //6.1 ����ҵ��ɵ��Ǹ�ʳ�˭��ʾʳ�this.element���ڱ���ʳ��ĺ��ӣ�ֻ��Ҫ���ڲ�ֵ�����жϼ���
      if (this.element) {
        //˵���Ѿ����оɵ�ʳ���ˣ�ɾ��
        this.map.removeChild(this.element);
      }
      //��ʳ�ﴴ�����������ҽ�����ʽ������
      //1 �����ṹ
      this.element = document.createElement("div");
      var div = this.element;//���棬���������дʹ��
      //2 ���û�����ʽ
      div.style.width = this.width + "px";
      div.style.height = this.height + "px";
      div.style.backgroundColor = this.bgColor;
      div.style.position = "absolute";//��λ
      //3 ��������������ֵ : Math.floor(Math.random()*��ͼ��/ʳ����ӿ�);
      this.x = Math.floor(Math.random() * this.map.offsetWidth / this.width);
      this.y = Math.floor(Math.random() * this.map.offsetHeight / this.height);
      //4 ����ȡ�����������λ�ã�����left��top����
      div.style.left = this.x * this.width + "px";
      div.style.top = this.y * this.height + "px";
      //5 ���뵽��ͼ��
      this.map.appendChild(div);
    };
    window.Food = Food;
  })();

  //С�ߵĹ��캯��
  (function () {

    function Snake(options) {
      //С�����������Ҫ��Щ�����أ�
      //����С�������е�ÿ�����Ӷ�����Ϊͨ���Ŀ�ߣ����Խ���ͳһ����
      this.width = options.width || 20;  //���
      this.height = options.height || 20;//�߶�
      //body���Ա������С��λ�õ�������Ϣ��ÿ�����ֵ������棬ÿ������������x��y����ɫ����
      this.body = [
        {x: 3, y: 7, bgColor: "red"},
        {x: 2, y: 7, bgColor: "orange"},
        {x: 1, y: 7, bgColor: "orange"}
      ];
      //С�߻���Ҫ��Щ���ԣ���ͼ��ʳ�����,�˶�����
      this.map = options.map;   //�����ͼ
      this.food = options.food; //����ʳ�����
      this.direction = options.direction || "right";  //�����˶��ķ���

      // ---- ���ڽ��е�ǰС������ṹ�ı��� ----
      this.elements = [];
    }

    //���ó�ʼ����������
    Snake.prototype.init = function () {

      // ---- �����������ɾ����ԭ�� ----
      //this.elements = [];
      //��һ�ִ���ִ�к��ڲ�Ӧ������3��div    init()
      //this.elements = ["div","div","div"];
      //�ڶ��ִ���ִ��ǰ���Ƚ��ڲ�ԭ�е�����div��Ӧ��ҳ��ṹ����ɾ�� move()
      //һ��ע�⣬ɾ������ҳ���е�div�ṹ������elements�еĶ����ǲ����κ�Ӱ���
      //this.elements =   ["div","div","div"];
      //�����ִ���ִ��ǰ��ҲҪ����ɾ������ʱthis.elementsӦ����6��div
      //this.elements = ["�ɵ�div", "�ɵ�div", "�ɵ�div", "�µ�div", "�µ�div", "�µ�div"];
      //��ִ��ɾ������ʱ�������˾ɵ�div��ִ��removeChild���ͻ���ֱ���

      // ---- 4 ɾ������ ----
      //�µ�С�߻��Ƴ����󣬾ɵ�С����Ȼ���ڣ�������ǰɾ������
      for (var i = this.elements.length - 1; i >= 0; i--) {
        //�ṹɾ��: removeChild()
        this.map.removeChild(this.elements[i]);
        //����ɾ��: ʹ�������splice������ ---- ��Ҫ��������ɾ����ԭ�������Ĳ������
        this.elements.splice(i, 1);
      }

      //console.log(this.elements);

      // ---- 1 �����Ҫ���оɽṹ��ɾ������Ҫ�ڴ���ʱ���Ƚ��б��� ----
      // ---- 2 ����С�ߵ������Ϊn�Ĳ��֣���һ��������ʽ�޷����棬��Ҫͨ������ṹ���д洢 ----
      //body�е����ж����ʾС�������е�ÿ�����֣�ÿ�����ֶ���һ�������ĺ��ӣ����б�������
      var body = this.body;//���ڱ���this.body��ֵ
      for (var i = 0; i < body.length; i++) {
        //body[i] - ��ʾ������ĳ�����ӵ�������Ϣ
        var div = document.createElement("div");
        div.style.width = this.width + "px";
        div.style.height = this.height + "px";
        div.style.backgroundColor = body[i].bgColor;
        div.style.position = "absolute";
        //����������ÿ�����ֵ�������Ϣ����С��ÿ�����ֵ�left��top����ֵ
        div.style.left = body[i].x * this.width + "px";
        div.style.top = body[i].y * this.height + "px";
        this.map.appendChild(div);//�����ͼ����ʾ

        // ---- 3 �ڴ���ʱ�������Ľṹ���浽this.elements�����У�����ɾ��ʱ�Ĳ��� ----
        this.elements.push(div);

      }
    };
    //����С���ƶ��Ĺ���
    Snake.prototype.move = function () {
      //���Ƿ��֣�С�����˶��Ĺ����У�ÿ���ƶ������޸�this.body��ÿ�������x��y��ֵ
      //�����ַ��֣�ÿ�����岿�����˶�һ��ʱ��Ҫ����Ϊ��ֵ��ǰһ������ǰ��λ��
      //��ͷ��������Ҫ�����˶��ķ�����е�������
      var body = this.body;

      //�����˸ı�֮ǰ����β���ӵ�����
      var lastX = body[body.length - 1].x;
      var lastY = body[body.length - 1].y;

      for (var i = body.length - 1; i > 0; i--) {
        //body[i]���ǳ�����ͷ�������������
        //x��y��Ҫ�޸�Ϊǰһ��Ķ�Ӧx��y
        body[i].x = body[i - 1].x;
        body[i].y = body[i - 1].y;
      }

      //����������ϣ�������ͷ������
      //��ͷ�������޸���Ҫ���ݵ�ǰ���˶�λ��
      switch (this.direction) {
        case "right":
          body[0].x++;
          break;
        case "left":
          body[0].x--;
          break;
        case "top":
          body[0].y--;
          break;
        case "bottom":
          body[0].y++;
      }

      // ---- ������ӵĹ��� ----
      //�˶���Ϻ󣬼���Ƿ�Ե���ʳ��
      //�Ƚ���Ϸ������д��ϣ���С�����������ٴ����������
      //ֻҪ�ж�ʳ����������ͷ�����꣬�����ȣ�˵���Ե���ʳ���ʱ���һ�����弴��
      if (body[0].x == this.food.x && body[0].y == this.food.y) {
        //��С�����һ������Ĳ��֣�ʵ���Ͼ��Ǹ�this.body�����һ���µĶ��󼴿�
        //�������ʹ����β���˶�֮ǰ�ľ�����(��Ҫ������������޸Ĵ���ǰ�Ƚ���һ�α���)
        body.push({
          bgColor: "orange",//ע�����ñ���ɫʱ���������ƣ���bgColor������
          x: lastX,
          y: lastY,
        });
        //������������µ�������������´���һ��ʳ��
        this.food.init();
      }


      //���������������ϣ�����С�ߵ�init�������µ��������»���һ���µ�С��
      //this.init();
      //������˼����ʽ��С������ı��Ӧ�����̽��л��Ʋ�����Ϊ�˱�����Ϸ����������С���߳�����ͼ������
      //���ǿ��Խ�init�������õ�Game��snakeRun��

    };
    //��Snake��¶��window����
    window.Snake = Snake;
  })();

  //������Ϸ���ƹ��캯��
  (function () {

    //��������Ϸ���ܵ��Ե��ú����з���
    var that;

    function Game(options) {
      //��Ҫʹ����Щ���ԣ���ͼ��С�ߣ�ʳ��
      this.map = options.map;
      this.food = new Food({map: this.map});//ֱ�ӽ���ʳ������ʵ��������
      this.snake = new Snake({map: this.map, food: this.food});//ʹ��snake����ֱ�ӽ���С�ߵ�ʵ��������������һ��ʵ������


      that = this;
    }

    //Game�ĳ�ʼ������
    Game.prototype.init = function () {
      //���ð�������
      this.changeDirection();
      //��ʼ��ʳ��
      this.food.init();
      //��ʼ��С��
      this.snake.init();
      //����С�ߵ��˶�
      this.snakeRun();
    };
    //Game����������С���˶��Ĵ���
    Game.prototype.snakeRun = function () {
      //���ö�ʱ������С�߼��һ��ʱ�����һ��move��������
      var timer = setInterval(function () {
        //ע�⣬��λ���Ƕ�ʱ���ڲ�����һ����ͨ�ĺ����ṹ��ʹ��thisָ����window����
        //console.log(this);//window
        //console.log(this.snake);//undefined
        //this.snake.move();//����
        //������Ҫthisָ��ͨ��Game������ʵ������
        //ͨ�����ⲿ���ж�this�ı�����������������λ�ÿ���ʹ��that����ȡ����ȷ��ֵ
        var she = that.snake;
        she.move();//�˴��޸ĺ�move�н����޸������꣬��û�н���С�ߵĻ��Ʋ���

        //������Ϸ�����ĳ�����
        //��⣬��ǰС���˶���λ��(��ͷ��λ�ã���������˵�ͼ�ı߽磬������Ϸ����);
        //���������ֵ�����ֵΪ39����ͼ��/����С����
        var maxX = that.map.offsetWidth / she.width;
        var maxY = that.map.offsetHeight / she.height;

        var sheHead = she.body[0];

        if (sheHead.x < 0 || sheHead.x > maxX - 1 || sheHead.y < 0 || sheHead.y > maxY - 1) {
          alert("��Ϸ����");
          clearInterval(timer);   //�����ʱ��
          return;                 //��������ֹ�����init������С�߻��Ƶ���ͼ���⡣
        }

        //��д�����λ�õ�Ŀ�ľ���Ϊ�����Ӿ�Ч���Ͽ���û������
        //���С�ߵ������ں���Χ�ڣ����л��ƣ�����ֱ���������if�н������д��룬�����л��Ʋ���
        she.init();

      }, 150);

    };

    //��Game��������һ�����������ڼ��̲���  keyup keydown
    Game.prototype.changeDirection = function () {
      //��document����keydown�¼�
      document.onkeydown = function (e) {
        //����жϣ���ǰ���µİ������ĸ������أ�
        var e = e || window.event;
        //�¼������keyCode���ԣ����Է���ĳ��������Ӧ��Ψһ�����룬��ֵ����
        //console.log(e.keyCode);
        //37 - ��    38 - ��   39 - ��   40 - ��
        var keyCode = e.keyCode;

        console.log(keyCode);

        //�����ǰdirection���Ե�ֵΪright�����û������37��ť����ֹ������˶������޸�
        if (that.snake.direction == "right" && keyCode == 37) {
          return;
        }


        switch (keyCode) {
          case 37:
            //����С�ߵ�direction����
            //�¼��ڲ���this����������Ҫ��ֵ������ʹ��֮ǰ���õ�that�������з���
            that.snake.direction = "left";
            break;
          case 38:
            that.snake.direction = "top";
            break;
          case 39:
            that.snake.direction = "right";
            break;
          case 40:
            that.snake.direction = "bottom";
            break;
        }
      };
    };

    //��Game��¶��window����
    window.Game = Game;
  })();


  //����Ĳ������룺
  var mapDiv = document.getElementById("map");
  var game1 = new Game({map: mapDiv});
  game1.init();


</script>
</body>
</html>