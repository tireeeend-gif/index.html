<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>Зазеркалье — мастерская</title>

  <!-- Шрифты -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700;800&family=Unbounded:wght@600;700&display=swap" rel="stylesheet">

  <!-- Библиотеки для QR и Сканера -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
  <script src="https://unpkg.com/html5-qrcode@2.3.8/html5-qrcode.min.js"></script>

  <style>
    :root {
      --bg: #090814;
      --card: rgba(28, 23, 48, 0.92);
      --card2: rgba(45, 34, 78, 0.8);
      --violet: #9b5cff;
      --pink: #ff4fa3;
      --cyan: #4eeeff;
      --gold: #ffd36a;
      --text: #f8f5ff;
      --muted: #b9b0cd;
      --green: #55e2aa;
      --danger: #ff6177;
    }

    * { box-sizing: border-box; }
    body {
      margin: 0;
      color: var(--text);
      background:
        radial-gradient(circle at 10% 5%, rgba(155, 92, 255, .32), transparent 24%),
        radial-gradient(circle at 90% 12%, rgba(255, 79, 163, .18), transparent 26%),
        radial-gradient(circle at 50% 95%, rgba(78, 238, 255, .14), transparent 30%),
        var(--bg);
      font-family: "Montserrat", sans-serif;
      min-height: 100vh;
    }

    button, input, select, textarea { font: inherit; }
    button { cursor: pointer; }

    .app {
      max-width: 520px;
      min-height: 100vh;
      margin: auto;
      position: relative;
      overflow: hidden;
      padding-bottom: 88px;
    }

    .app::before {
      content: "";
      position: fixed;
      inset: 0;
      pointer-events: none;
      opacity: .18;
      background-image:
        linear-gradient(rgba(255,255,255,.04) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,.04) 1px, transparent 1px);
      background-size: 28px 28px;
    }

    .hidden { display: none !important; }

    .welcome {
      min-height: 100vh;
      max-width: 520px;
      margin: auto;
      padding: 34px 20px;
      display: flex;
      flex-direction: column;
      justify-content: center;
      position: relative;
    }

    .brand {
      display: flex;
      gap: 14px;
      align-items: center;
      margin-bottom: 26px;
    }

    .logo {
      width: 66px;
      height: 66px;
      border-radius: 20px;
      background: linear-gradient(135deg, var(--violet), var(--pink));
      box-shadow: 0 0 34px rgba(155,92,255,.55);
      display: grid;
      place-items: center;
    }

    .logo svg { width: 48px; height: 48px; }
    .brand h1 {
      margin: 0;
      font-family: "Unbounded", sans-serif;
      font-size: 22px;
      line-height: 1.1;
    }

    .brand p {
      color: var(--muted);
      font-size: 11px;
      margin: 6px 0 0;
      text-transform: uppercase;
      letter-spacing: 1px;
    }

    .hero {
      padding: 23px;
      border-radius: 30px;
      background: linear-gradient(145deg, rgba(60,38,107,.9), rgba(18,15,34,.95));
      border: 1px solid rgba(255,255,255,.11);
      box-shadow: 0 22px 55px rgba(0,0,0,.3);
      margin-bottom: 18px;
    }

    .hero h2 {
      font-family: "Unbounded", sans-serif;
      margin: 0 0 12px;
      font-size: 23px;
      line-height: 1.4;
    }

    .hero h2 span { color: var(--cyan); }
    .hero p { color: var(--muted); line-height: 1.55; font-size: 14px; }

    .features {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 11px;
      margin-bottom: 24px;
    }

    .feature {
      background: rgba(255,255,255,.06);
      padding: 14px;
      border-radius: 18px;
      border: 1px solid rgba(255,255,255,.08);
      color: var(--muted);
      font-size: 12px;
    }

    .feature b { display: block; font-size: 24px; color: white; margin-bottom: 5px; }

    .btn {
      width: 100%;
      border: 0;
      border-radius: 16px;
      color: #fff;
      padding: 15px 17px;
      font-weight: 800;
      transition: .2s;
    }

    .btn:active { transform: scale(.97); }
    .btn-main {
      background: linear-gradient(90deg, var(--violet), var(--pink));
      box-shadow: 0 10px 24px rgba(216, 76, 190, .28);
    }

    .btn-dark {
      margin-top: 10px;
      background: rgba(255,255,255,.09);
      border: 1px solid rgba(255,255,255,.12);
    }

    .topbar {
      padding: 17px 18px 12px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      position: sticky;
      top: 0;
      z-index: 5;
      background: linear-gradient(to bottom, rgba(9,8,20,.96), rgba(9,8,20,.7), transparent);
      backdrop-filter: blur(9px);
    }

    .top-brand { display: flex; align-items: center; gap: 10px; }
    .top-brand .logo { width: 45px; height: 45px; border-radius: 14px; }
    .top-brand .logo svg { width: 35px; height: 35px; }
    .top-brand strong { font-family: "Unbounded"; font-size: 13px; }
    .top-brand small { display: block; color: var(--cyan); font-size: 10px; margin-top: 4px; }

    .icon-btn {
      border: 1px solid rgba(255,255,255,.12);
      background: rgba(255,255,255,.07);
      width: 42px;
      height: 42px;
      color: #fff;
      border-radius: 14px;
      font-size: 20px;
    }

    main { padding: 8px 16px 20px; }
    .screen-title { margin: 10px 0 18px; font-family: "Unbounded"; font-size: 20px; }
    .subtitle { color: var(--muted); font-size: 12px; margin-top: -11px; margin-bottom: 17px; }

    .card {
      background: var(--card);
      border: 1px solid rgba(255,255,255,.1);
      border-radius: 23px;
      padding: 17px;
      margin-bottom: 14px;
      box-shadow: 0 12px 28px rgba(0,0,0,.16);
    }

    .profile-card {
      background: linear-gradient(135deg, rgba(122,72,214,.75), rgba(242,62,151,.6));
      position: relative;
      overflow: hidden;
    }

    .profile-card::after {
      content: "🐆";
      position: absolute;
      right: -4px;
      bottom: -20px;
      font-size: 115px;
      opacity: .16;
      transform: rotate(-13deg);
    }

    .profile-row { display: flex; align-items: center; gap: 14px; position: relative; z-index: 1; }
    .avatar {
      width: 70px;
      height: 70px;
      border-radius: 22px;
      object-fit: cover;
      background: #271c45;
      border: 2px solid rgba(255,255,255,.56);
    }

    .profile-row h2 { margin: 0 0 5px; font-size: 18px; }
    .profile-row p { margin: 0; font-size: 12px; opacity: .9; }
    .badge {
      display: inline-block;
      margin-top: 7px;
      padding: 5px 9px;
      border-radius: 20px;
      font-size: 10px;
      font-weight: 800;
      background: rgba(0,0,0,.24);
    }

    .id-box {
      margin-top: 17px;
      position: relative;
      z-index: 1;
      background: rgba(0,0,0,.22);
      border-radius: 15px;
      padding: 11px 13px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .id-box small { opacity: .76; display: block; font-size: 10px; }
    .id-box b { font-size: 14px; letter-spacing: .7px; }

    .quick-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 11px; }
    .quick {
      border: 0;
      text-align: left;
      color: white;
      padding: 16px;
      border-radius: 19px;
      background: var(--card2);
      border: 1px solid rgba(255,255,255,.1);
    }

    .quick .emoji { font-size: 28px; display: block; margin-bottom: 9px; }
    .quick b { font-size: 13px; }
    .quick small { display: block; margin-top: 4px; color: var(--muted); font-size: 10px; }

    .news-card { border-left: 4px solid var(--pink); }
    .news-card h3 { font-size: 14px; margin: 0 0 7px; }
    .news-card p { color: var(--muted); font-size: 12px; line-height: 1.5; margin: 0; }
    .date { color: var(--cyan); font-size: 10px; margin-bottom: 8px; font-weight: 700; }

    .bottom-nav {
      position: fixed;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%);
      width: min(520px, 100%);
      z-index: 10;
      display: flex;
      justify-content: space-around;
      background: rgba(17,13,31,.94);
      border-top: 1px solid rgba(255,255,255,.1);
      backdrop-filter: blur(15px);
      padding: 9px 5px 13px;
    }

    .nav-btn {
      color: #a9a0bb;
      background: transparent;
      border: 0;
      font-size: 10px;
      min-width: 58px;
    }

    .nav-btn span { font-size: 21px; display: block; margin-bottom: 3px; }
    .nav-btn.active { color: var(--cyan); }

    label {
      color: #d7cfe7;
      font-size: 11px;
      font-weight: 700;
      display: block;
      margin: 13px 0 6px;
    }

    input, select, textarea {
      width: 100%;
      padding: 13px;
      color: #fff;
      outline: none;
      border-radius: 13px;
      background: rgba(255,255,255,.07);
      border: 1px solid rgba(255,255,255,.12);
    }

    select option { background: #1c1630; }
    textarea { min-height: 90px; resize: vertical; }

    .photo-upload {
      width: 105px;
      height: 105px;
      margin: 4px auto 15px;
      border-radius: 28px;
      overflow: hidden;
      position: relative;
      border: 2px dashed var(--cyan);
      background: rgba(78,238,255,.08);
      display: grid;
      place-items: center;
    }

    .photo-upload img { width: 100%; height: 100%; object-fit: cover; }
    .photo-upload span { font-size: 33px; }
    .photo-upload input { position: absolute; inset: 0; opacity: 0; cursor: pointer; }

    .direction-item, .teacher-item, .schedule-item, .chat-item {
      display: flex;
      gap:
