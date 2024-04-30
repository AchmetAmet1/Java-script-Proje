<!DOCTYPE html>
<html lang="tr">
<head>
    
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Meyve Toplama Macerası</title>
  
    <style>
        body {
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;/*Genel olarak flexleri verdim uzunluk arka plan resmi  */
    align-items: center;
    height: 100vh;
    background-image: url("arka plan.jpg");
    background-size: cover;
    background-position: center;
}

.game-container {
    position: relative;/* Buradaysa oyunun web sitesinde ne kadar yer kaplayacığnı verdim*/
    width: 1500px;
    height: 700px;
    border: 2px solid #000;
}

.character {
    position: absolute;
    width: 50px;
    height: 50px;
    background-color: transparent;
    bottom: 0;
    left: 0;
    /* Başlangıçta kutu sol kenara hizalanır */
    transition: left 0.5s ease;
    transition: right 0.5s ease;
}

.fruit {
    position: absolute; /*Burada meyveyi tanımladım büyüklüğünü ve pozisyonu ve hareketini de ekledim */
    width: 30px;
    height: 30px;
    background-color: transparent;
    bottom: 100%;
    left: 100%  ;
    transform: translateX(-50%);
    animation: moveFruit 2s infinite alternate;
}

@keyframes moveCharacter {
    from { left: 0; }
    to { left: 350px; }
    from{ left: 0;}
    to{right: 0; }
}

@keyframes moveFruit {
    from { bottom: 100%; }
    to { bottom: 0; }
}


    </style>
</head>
<body>
    <h1 align="center">Meyve Oyunu</h1>
    <div class="game-container">
        <div class="character" id="character"><!-- Burada classları yazdım sepet.png resmininin büyüklüğünü ve uzunluğunu yazdım -->
            <img src="sepet.png" alt="sepet resmi" width="70" height="70"> 
        </div>
        <div class="fruit" id="fruit">
            <img src="armut.png" alt="armut resmi" width="50" height="50" ><!-- Aynı şekilde armut.png resmi içinde geçerli bir şekilde yine yazdım. -->
        </div>
    </div>
    <div class="score" id="score"><h1>Skor: 0</h1></div>
    <div id="oyun-alani">
        <div id="kutu"></div>
    </div>
    
    
    <button onclick="gosterKurallar()">Kurallar</button>

    <script>

function gosterKurallar() {
            alert("Oyunumun Kuralları:Amacımız oyunumuzda olan sepet ile düşen armutu toplamak ve skor kazanmak.Fakat meyveyi 4 kezden fazla yakalayamassak oyunu kaybederiz bu yüzden dikkatli bir şekilde ve hata yapmadan oynamaya çalışın.İyi Eğlenceler!");
        }

    const character = document.getElementById('character');//burada alinacak karakter meyve gibi seyleri aliyorum.
    const fruit = document.getElementById('fruit');
    const scoreDisplay = document.getElementById('score');
    let score = 0;

document.addEventListener('keydown', function(event) {
    if (event.key === 'ArrowLeft') {
        moveCharacter(-50);
    } else if (event.key === 'ArrowRight') {//bu fonksiyonda sol ok tuşuna bastığımda -50 olarak sola hareket etmesini sağ ok tuşuna bastığımdaysa +50 olarak sağ tarafa doğru hareket etmesini sağladım.
        moveCharacter(50);
    }
});

function moveCharacter(distance) {
    const characterPosition = parseInt(window.getComputedStyle(character).getPropertyValue('left'));
    const newPosition = characterPosition + distance;

    if (newPosition >= 0 && newPosition <= 1450) {
        character.style.left = newPosition + 'px';//burada gidebilecegi pozisyonlari ve sınırını belirliyorum.
    }

    checkCollision();
}

function dropFruit() {
    const randomPosition = Math.floor(Math.random() * 1450);
    fruit.style.left = randomPosition + 'px';//burada fruit(yani armutun)random bir şekilde spawnlanmasını istedim.
    fruit.style.bottom = '100px';
}

function checkCollision() {
    const characterRect = character.getBoundingClientRect();
    const fruitRect = fruit.getBoundingClientRect();

    if (
        characterRect.left < fruitRect.right &&
        characterRect.right > fruitRect.left &&
        characterRect.top < fruitRect.bottom &&
        characterRect.bottom > fruitRect.top
    ) {
        score++;
        scoreDisplay.textContent = 'Skor: ' + score;// her meyve yakalandiginda skorumuz 1 puan artar ve yaninda yazar.
        dropFruit();
        let fruitMissed = 0;
    } else {
                fruitMissed++; // Meyve kaçırıldığında arttır
                if (fruitMissed >= 2) {
                    // Meyveye 2 kez dokunulmadığında oyunu durdur
                    alert("Oyun bitti! Meyveye 4 kez dokunamadınız.Yeniden oynamak için yeniden başlayınız.");
                    fruitMissed = 0; // Oyunu tekrar başlamak için sıfırla
}
    }
}


function moveFruit() {
    const fruitBottom = parseInt(window.getComputedStyle(fruit).getPropertyValue('bottom'));
    
    if (fruitBottom > 0) {
        fruit.style.bottom = (fruitBottom - 5) + 'px';/* burada dusmesi icin gereken kodu yazdim. */
    } else {
        dropFruit();
    }

    setTimeout(moveFruit, 50);
}

    dropFruit();
    moveFruit();



    </script>



</body>
</html>






![oyunun görünüşü](https://github.com/AchmetAmet1/Java-script-Proje/assets/168425092/c44875cf-3ee1-44e1-9df0-fe452c368c82)
![oyunun kuralları](https://github.com/AchmetAmet1/Java-script-Proje/assets/168425092/3136761f-0e04-4319-aa3d-28ea5d5d65fd)
