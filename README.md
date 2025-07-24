<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>New Jeans</title>
    <link rel="stylesheet" href="css/style.css">
    <style>
        /* スライドショーのコンテナ */
        .slideshow-container {
            position: relative;
            max-width: 100%;
            margin: auto;
        }

        /* 各スライドの基本スタイル */
        .slide {
            display: none;
        }

        /* フェードインアニメーション */
        .fade {
            animation-name: fade;
            animation-duration: 1.5s;
        }

        @keyframes fade {
            from {
                opacity: 0;
            }
            to {
                opacity: 1;
            }
        }

        /* コメント表示エリア */
        .comment-item {
            border-bottom: 1px solid #ccc;
            margin-bottom: 10px;
            padding-bottom: 10px;
        }

        .comment-item p {
            margin: 5px 0;
        }

        .comment-item .name {
            font-weight: bold;
        }

        /* フッターのスタイル */
        footer {
            background-color: #333;
            color: white;
            padding: 20px;
            text-align: center;
        }

        footer .footer-content {
            max-width: 1200px;
            margin: 0 auto;
        }

        footer nav ul {
            list-style-type: none;
            padding: 0;
        }

        footer nav ul li {
            display: inline;
            margin-right: 10px;
        }

        footer nav ul li a {
            color: white;
            text-decoration: none;
        }

        footer nav ul li a:hover {
            text-decoration: underline;
        }

        footer .footer-content a {
            color: white;
            text-decoration: none;
        }

        footer .footer-content a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>

<!-- ロゴ -->
<div><a href="https://www.newjeans.jp"><img src="images/logo.png" width="600" alt="New Jeans Logo"></a></div>

<!-- スライドショー -->
<div class="slideshow-container">
    <div class="slide fade">
        <img src="images/omg.png" alt="New Jeans group performing" style="width:100%">
    </div>
    <div class="slide fade">
        <img src="images/logo-whale.png" alt="Whale logo" style="width:100%">
    </div>
    <div class="slide fade">
        <img src="images/new.png" alt="New Jeans group" style="width:100%">
    </div>
</div>

<h2>MENU</h2>

<!-- ヘッダー -->
<header>
    <nav class="nav">
        <ul class="menu-group">
            <li class="menu-item"><a href="access.html">PROFILE</a></li>
            <li class="menu-item"><a href="information.html">MUSIC</a></li>
        </ul>
    </nav>  
</header>

<main>
<h1>Newjeansとは</h1>
<ul>
    <li>
        NewJeansは、新レーベル『ADOR』よりデビューした最初のアーティストです。
    </li>
    <li>
        少女時代やf(x)、Red Velvetなど、名だたるアーティストの作品に携わってきたミン・ヒジン氏がグループ制作を指揮しました。
        <a href="https://www.newjeans.jp/">New jeans</a>
    </li>
</ul>

<h2>K‐POPアイドル</h2>

<h3>日本デビュー曲は『How sweet』</h3>

<div class="logo"><img src="images/logo-whale.png" alt="Whale Logo"></div>

<!-- コメントセクション -->
<h2>【コメント】</h2>

<!-- コメントフォーム -->
<form id="commentForm">
    <label for="name">お名前:</label>
    <input type="text" id="name" name="name" required>

    <label for="comment">コメント:</label>
    <textarea id="comment" name="comment" rows="4" required></textarea>

    <button type="submit">コメントを送信</button>
</form>

<!-- コメント一覧 -->
<h3>コメント一覧</h3>
<div id="commentList">
    <!-- コメントがここに表示されます -->
</div>

<script>
// スライドショーのロジック
let slideIndex = 0;

function showSlides() {
    let slides = document.getElementsByClassName("slide");

    // すべてのスライドを非表示にする
    for (let i = 0; i < slides.length; i++) {
        slides[i].style.display = "none";  
    }

    slideIndex++;
    if (slideIndex > slides.length) {
        slideIndex = 1;  // 最初のスライドに戻る
    }

    slides[slideIndex - 1].style.display = "block";  // 現在のスライドを表示
    setTimeout(showSlides, 3000);  // 3秒ごとにスライドを変更
}

// ページ読み込み時にスライドショーを開始
document.addEventListener("DOMContentLoaded", showSlides);

// コメントを追加する関数
document.getElementById("commentForm").addEventListener("submit", function(event) {
    event.preventDefault(); // フォームの送信を防ぐ

    // 入力された名前とコメントを取得
    const name = document.getElementById("name").value;
    const comment = document.getElementById("comment").value;

    // コメントを表示するエリアに新しいコメントを追加
    const commentList = document.getElementById("commentList");

    const commentItem = document.createElement("div");
    commentItem.classList.add("comment-item");

    commentItem.innerHTML = ` 
        <p class="name">${name}</p>
        <p>${comment}</p>
    `;

    commentList.appendChild(commentItem);

    // 入力フォームをリセット
    document.getElementById("commentForm").reset();
});
</script>

</main>

<!-- フッター -->
<footer>
    <div class="footer-content">
        <p>&copy; 2025 New Jeans. </p>
      
        <div>
            <p>フォローする:</p>
           
            <a href="https://www.instagram.com/newjeans_official/" target="_blank">Instagram</a> |
            <a href="https://www.youtube.com/@NewJeans_official" target="_blank">YouTube</a>
        </div>
    </div>
</footer>

</body>
</html>
