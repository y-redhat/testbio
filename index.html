<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>ビオトープ:食料リソース制限捕食系シミュレータ</title>
  <style>
    body { background: #222; color: #fff; text-align: center; }
    canvas { background: #111; border: 1px solid #888; margin: 1em auto; display: block; }
    h1 { font-family: 'Arial Black', sans-serif; margin: 1em 0 0.3em; }
    p { margin-bottom: 0.2em; }
    #command { margin: 0.5em; font-size:1.2em; width: 270px; }
  </style>
</head>
<body>
  <h1>コンピュータービオトープ</h1>
  <p>オレンジ＝獲物（prey）, 赤＝捕食者（predator）, 緑＝食べ物（food）<br>
    環境は増えすぎると自滅します。追加したいときは下のコマンド欄に：<br>
    <code>add prey</code>, <code>add predator</code>, <code>add food</code> で任意に補充！</p>
  <canvas id="biotope" width="500" height="500"></canvas>
  <div>
    <input type="text" id="command" placeholder="コマンド例: add prey" autocomplete="off">
  </div>
  <script>
    // --- 設定 ---
    const WIDTH = 500, HEIGHT = 500;
    const PREY_RADIUS = 7, PREDATOR_RADIUS = 11, FOOD_RADIUS = 3;
    const PREY_MAX = 999, PREDATOR_MAX = 999;
    const PREY_INIT = 10, PREDATOR_INIT = 2; // 最初はギリギリバランス
    const FOOD_INIT = 20;
    const FOOD_AUTO_ADD = 6;  // 放置すると全体でじわじわ枯渇しやすい

    const PREY_EN_MAX = 34, PREY_EN_REP = 22, PREY_EN_FOOD = 8, PREY_LIFE = 750, PREY_REP_PROB=0.08, PREY_EN_MOVE = 0.39;
    const PREDATOR_EN_MAX = 44, PREDATOR_EN_REP = 31, PREDATOR_EN_PREY = 16, PREDATOR_LIFE = 990, PREDATOR_REP_PROB=0.04, PREDATOR_EN_MOVE = 0.51;
    const FOOD_CAP = 44;

    // --- ユーティリティ
    function randXY() {
      return [Math.random()*(WIDTH-30)+15, Math.random()*(HEIGHT-30)+15];
    }
    function clamp(val, min, max) {
      return Math.max(min, Math.min(max, val));
    }
    function distance(x1,y1,x2,y2){ return Math.hypot(x1-x2,y1-y2); }

    // --- クラス定義
    class Prey {
      constructor(x, y, en=16) {
        this.x = x; this.y = y;
        this.en = en; this.age = 0;
      }
      move() {
        const theta = Math.random()*6.28;
        const sp = Math.random()*1.8+0.93;
        this.x = clamp(this.x + Math.cos(theta)*sp, PREY_RADIUS, WIDTH-PREY_RADIUS);
        this.y = clamp(this.y + Math.sin(theta)*sp, PREY_RADIUS, HEIGHT-PREY_RADIUS);
        this.en -= PREY_EN_MOVE;
        this.age++;
      }
      eat(foodArr) {
        for(let i=foodArr.length-1; i>=0; i--) {
          const f=foodArr[i];
          if(distance(this.x,this.y, f.x,f.y)<PREY_RADIUS+FOOD_RADIUS) {
            this.en = Math.min(PREY_EN_MAX, this.en+PREY_EN_FOOD);
            foodArr.splice(i,1); break;
          }
        }
      }
      canReproduce() { return this.en > PREY_EN_REP && Math.random()<PREY_REP_PROB;}
      reproduce() {
        this.en = Math.floor(this.en/2);
        const th = Math.random()*6.28, dist = PREY_RADIUS*1.7;
        return new Prey(
          clamp(this.x + Math.cos(th)*dist, PREY_RADIUS, WIDTH-PREY_RADIUS),
          clamp(this.y + Math.sin(th)*dist, PREY_RADIUS, HEIGHT-PREY_RADIUS),
          this.en
        );
      }
      isDead() { return this.en<=0 || this.age>PREY_LIFE;}
    }

    class Predator {
      constructor(x, y, en=22) {
        this.x = x; this.y = y;
        this.en = en; this.age = 0;
      }
      move(preys) {
        // 近傍の獲物を簡易追尾
        let target=null, mind=140;
        for(const p of preys) {
          let d=distance(this.x,this.y,p.x,p.y);
          if(d<mind) { mind=d; target=p; }
        }
        let theta;
        if(target && mind<190) {
          theta = Math.atan2(target.y-this.y, target.x-this.x) + (Math.random()-0.5)*0.5;
        } else {
          theta = Math.random()*6.28;
        }
        const sp = Math.random()*1.7+1.2;
        this.x = clamp(this.x + Math.cos(theta)*sp, PREDATOR_RADIUS, WIDTH-PREDATOR_RADIUS);
        this.y = clamp(this.y + Math.sin(theta)*sp, PREDATOR_RADIUS, HEIGHT-PREDATOR_RADIUS);
        this.en -= PREDATOR_EN_MOVE;
        this.age++;
      }
      eat(preys) {
        for(let i=preys.length-1; i>=0; i--) {
          const p=preys[i];
          if(distance(this.x,this.y,p.x,p.y)<PREDATOR_RADIUS+PREY_RADIUS) {
            this.en = Math.min(PREDATOR_EN_MAX, this.en+PREDATOR_EN_PREY);
            preys.splice(i,1); break;
          }
        }
      }
      canReproduce() { return this.en > PREDATOR_EN_REP && Math.random()<PREDATOR_REP_PROB;}
      reproduce() {
        this.en=Math.floor(this.en/2);
        const th=Math.random()*6.28, dist=PREDATOR_RADIUS*1.5;
        return new Predator(
          clamp(this.x+Math.cos(th)*dist,PREDATOR_RADIUS,WIDTH-PREDATOR_RADIUS),
          clamp(this.y+Math.sin(th)*dist,PREDATOR_RADIUS,HEIGHT-PREDATOR_RADIUS),
          this.en
        );
      }
      isDead() { return this.en<=0 || this.age>PREDATOR_LIFE; }
    }

    class Food {
      constructor(x, y) { this.x = x; this.y = y; }
    }

    // --- 集合
    /** @type {Prey[]} */ let preys = [];
    /** @type {Predator[]} */ let predators = [];
    /** @type {Food[]} */ let foods = [];

    // --- 初期化
    function resetAll() {
      preys = [];
      predators = [];
      foods = [];
      for(let i=0;i<PREY_INIT;i++){ const [x,y]=randXY(); preys.push(new Prey(x,y)); }
      for(let i=0;i<PREDATOR_INIT;i++){ const [x,y]=randXY(); predators.push(new Predator(x,y)); }
      for(let i=0;i<FOOD_INIT;i++){ const [x,y]=randXY(); foods.push(new Food(x,y)); }
    }

    function addFood(n=1) {
      for(let i=0;i<n;i++) {
        if(foods.length<FOOD_CAP) { const [x,y]=randXY(); foods.push(new Food(x,y)); }
      }
    }
    function addPrey(n=1) {
      for(let i=0;i<n;i++){
        if(preys.length<PREY_MAX) { const [x,y]=randXY(); preys.push(new Prey(x,y,12+Math.random()*8)); }
      }
    }
    function addPredator(n=1) {
      for(let i=0;i<n;i++){
        if(predators.length<PREDATOR_MAX) { const [x,y]=randXY(); predators.push(new Predator(x,y,12+Math.random()*8)); }
      }
    }

    // --- コマンド処理
    const cmd=document.getElementById('command');
    cmd.addEventListener('keydown',e=>{
      if(e.key==="Enter") {
        let v=cmd.value.trim().toLowerCase();
        if(v==="add prey") addPrey();
        else if(v==="add predator") addPredator();
        else if(v==="add food") addFood();
        // おまけ：一度に複数追加（例: add prey 5）
        else if(v.match(/^add prey (\d+)$/)) addPrey(Number(RegExp.$1));
        else if(v.match(/^add predator (\d+)$/)) addPredator(Number(RegExp.$1));
        else if(v.match(/^add food (\d+)$/)) addFood(Number(RegExp.$1));
        cmd.value="";
      }
    });

    // --- 描画と進行
    const canvas = document.getElementById('biotope');
    const ctx = canvas.getContext('2d');
    function draw() {
      ctx.clearRect(0,0,WIDTH,HEIGHT);

      // 食べ物
      for(const f of foods) {
        ctx.beginPath();
        ctx.arc(f.x,f.y,FOOD_RADIUS,0,6.28);
        ctx.fillStyle="#46f067"; ctx.globalAlpha=0.85; ctx.fill();
      }
      ctx.globalAlpha=1;
      // 獲物
      for(const c of preys) {
        ctx.beginPath();
        ctx.arc(c.x,c.y,PREY_RADIUS,0,6.28);
        const l=Math.min(1,c.en/PREY_EN_MAX)*0.8+0.2;
        ctx.fillStyle = `rgba(${230*l},${146*(1-l)+80},22,0.9)`;
        ctx.shadowBlur = 7*l;
        ctx.shadowColor = "#f5a042";
        ctx.fill();
      }
      ctx.shadowBlur = 0;
      // 捕食者
      for(const pr of predators) {
        ctx.beginPath();
        ctx.arc(pr.x,pr.y,PREDATOR_RADIUS,0,6.28);
        const l=Math.min(1,pr.en/PREDATOR_EN_MAX)*0.9+0.1;
        ctx.fillStyle = `rgba(${200*l+55},25,39,0.85)`;
        ctx.strokeStyle = "#ff5555";
        ctx.lineWidth = 1.7;
        ctx.fill();
        ctx.stroke();
      }

      // 絶滅表示
      if(preys.length===0) {
        ctx.font="23px sans-serif"; ctx.fillStyle="#ff4";
        ctx.fillText("獲物:絶滅(増やすには add prey)", WIDTH/2-110, HEIGHT/2-10);
      }
      if(predators.length===0) {
        ctx.font="18px sans-serif"; ctx.fillStyle="#fa4";
        ctx.fillText("捕食者:絶滅(add predatorで追加)", WIDTH/2-105, HEIGHT/2+15);
      }
    }

    function update() {
      // 食料自動補充（増殖しすぎると補えなくなる）  
      if(foods.length<FOOD_CAP) addFood(Math.max(0, Math.min(FOOD_AUTO_ADD, FOOD_CAP-foods.length)));

      // --- 獲物 ---
      let babies = [];
      for(const c of preys) {
        c.move(); c.eat(foods);
        if(c.canReproduce() && preys.length+babies.length<PREY_MAX) babies.push(c.reproduce());
      }
      preys = preys.filter(c=>!c.isDead()).concat(babies);

      // --- 捕食者 ---
      let predBabies=[];
      for(const pr of predators) {
        pr.move(preys); pr.eat(preys);
        if(pr.canReproduce() && predators.length+predBabies.length<PREDATOR_MAX) predBabies.push(pr.reproduce());
      }
      predators = predators.filter(pr=>!pr.isDead()).concat(predBabies);
    }

    resetAll();
    function loop() {
      update(); draw();
      requestAnimationFrame(loop);
    }
    loop();

    // canvasクリックで全リセット
    canvas.addEventListener('click', resetAll);
  </script>
</body>
</html>
