<!DOCTYPE html>
<html lang="my">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ယွန်းရတီဦး(Enhanced UI)</title>
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
    
    <style>
        html, body {
            height: 100%;
            margin: 0;
            padding: 0;
            overflow: hidden;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #e5ddd5;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .chat-container {
            width: 100%; /* Full Width */
            height: 100%; /* Full Height */
            max-width: none; /* No max-width restriction */
            display: flex;
            flex-direction: column;
            overflow: hidden;
            padding-bottom: env(safe-area-inset-bottom, 0); 
            background-color: #fff;
        }

        .chat-header {
            background-color: #0088cc;
            color: white;
            padding: 10px 15px;
            display: flex;
            align-items: center;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            z-index: 10;
            flex-shrink: 0;
            position: relative; 
            transition: background-color 0.2s ease;
        }
        .chat-header:hover {
            background-color: #007bb5; 
        }

        /* Target the clickable profile info area */
        .chat-header .profile-info-area {
            display: flex;
            align-items: center;
            flex-grow: 1; /* Allows it to take available space */
            cursor: pointer; /* Add cursor pointer specifically here */
            padding-right: 10px; /* Some padding to avoid hitting icons */
        }

        .chat-header .profile-pic {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background-color: #f0f0f0;
            margin-right: 10px;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5em; 
            box-shadow: 0 0 0 2px rgba(255,255,255,0.3); 
        }
        .chat-header .profile-pic img {
            width: 100%;
            height: 100%;
            object-fit: cover; 
            display: block; 
        }
        .chat-header .info {
            flex-grow: 1;
        }
        .chat-header .info h3 {
            margin: 0;
            font-weight: 500;
            font-size: 1.1em;
        }
        .chat-header .info p {
            margin: 0;
            font-size: 0.8em;
            opacity: 0.8;
        }
        .chat-header .icons {
            display: flex;
            gap: 15px;
        }
        .chat-header .icons .material-icons {
            font-size: 1.4em;
            cursor: pointer;
            padding: 5px;
            border-radius: 50%;
            transition: background-color 0.2s ease;
        }
        .chat-header .icons .material-icons:hover {
            background-color: rgba(255,255,255,0.2);
        }

        /* More_vert Menu အတွက် CSS (မပြောင်းလဲပါ) */
        .header-menu {
            position: absolute;
            top: 100%;
            right: 10px;
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.2);
            min-width: 200px;
            z-index: 20;
            opacity: 0;
            visibility: hidden;
            transform: translateY(-10px);
            transition: opacity 0.2s ease-out, transform 0.2s ease-out, visibility 0.2s ease-out;
            padding: 8px 0;
        }

        .header-menu.active {
            opacity: 1;
            visibility: visible;
            transform: translateY(0);
        }

        .header-menu ul {
            list-style: none;
            padding: 0;
            margin: 0;
        }

        .header-menu ul li {
            padding: 12px 20px;
            color: #333;
            cursor: pointer;
            font-size: 0.95em;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .header-menu ul li .material-icons {
            font-size: 20px;
            color: #666;
        }

        .header-menu ul li:hover {
            background-color: #f0f0f0;
        }
        .header-menu ul li:active {
            background-color: #e0e0e0;
        }

        /* Search Bar အတွက် CSS (မပြောင်းလဲပါ) */
        .search-bar {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: #0088cc;
            display: flex;
            align-items: center;
            padding: 0 15px;
            box-sizing: border-box;
            transform: translateX(100%);
            transition: transform 0.3s ease-in-out;
            z-index: 15;
        }

        .search-bar.active {
            transform: translateX(0);
        }

        .search-bar input {
            flex-grow: 1;
            padding: 8px 12px;
            border: none;
            border-radius: 20px;
            outline: none;
            font-size: 1em;
            background-color: rgba(255, 255, 255, 0.2);
            color: white;
            margin-right: 10px;
        }
        .search-bar input::placeholder {
            color: rgba(255, 255, 255, 0.7);
        }

        .search-bar .material-icons {
            color: white;
            font-size: 1.5em;
            cursor: pointer;
            margin-left: 5px;
        }
        .search-bar .search-count {
            color: white;
            font-size: 0.9em;
            margin: 0 10px;
            white-space: nowrap;
        }
        .search-bar .search-nav-buttons {
            display: flex;
            gap: 5px;
        }
        .search-bar .search-nav-buttons .material-icons {
            font-size: 1.4em;
            cursor: pointer;
            padding: 5px;
            border-radius: 50%;
            transition: background-color 0.2s;
        }
        .search-bar .search-nav-buttons .material-icons:hover {
            background-color: rgba(255, 255, 255, 0.1);
        }


        .chat-messages {
            flex-grow: 1;
            padding: 15px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 8px;
            background-image: url('https://user-images.githubusercontent.com/15075759/28719144-86dc0f70-73b1-11e7-911d-60d70fcded21.png');
            background-size: cover;
            background-position: center;
        }

        /* General message container */
        .message-container {
            display: flex;
            align-items: flex-start; 
            gap: 8px; 
            margin-bottom: 5px; 
            position: relative; /* For reaction buttons positioning */
            padding-bottom: 10px; /* Space for reactions */
        }

        /* Bot's profile pic for messages */
        .message-container .message-profile-pic {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            overflow: hidden;
            background-color: #f0f0f0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2em;
            flex-shrink: 0; 
        }
        .message-container .message-profile-pic img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            display: block;
        }

        .message-container.bot-container {
            align-self: flex-start;
            flex-direction: row; 
        }

        .message-container.user-container {
            align-self: flex-end;
            flex-direction: row-reverse; 
        }
        /* User message profile pic is hidden by default */
        .message-container.user-container .message-profile-pic {
            display: none; 
        }

        .message {
            max-width: 80%; 
            padding: 6px 12px; 
            border-radius: 10px; 
            word-wrap: break-word;
            box-shadow: 0 1px 0.5px rgba(0, 0, 0, 0.13);
            position: relative;
            font-size: 0.95em;
            flex-grow: 0; 
            flex-shrink: 1; 
        }

        /* Tail for message bubbles */
        .message::after {
            content: '';
            position: absolute;
            bottom: 0px; 
            width: 0;
            height: 0;
            border: 8px solid transparent;
        }

        .user-message {
            background-color: #dcf8c6;
            color: #333;
            border-bottom-right-radius: 2px; /* For the tail */
        }
        .user-message::after {
            border-left-color: #dcf8c6;
            right: -7px;
            transform: translateY(50%) rotate(45deg);
        }

        .bot-message {
            background-color: rgba(255, 255, 255, 0.5); /* 50% opacity white */
            color: #333;
            border-bottom-left-radius: 2px; /* For the tail */
        }
        .bot-message::after {
            border-right-color: rgba(255, 255, 255, 0.5); /* 50% opacity white */
            left: -7px;
            transform: translateY(50%) rotate(-45deg);
        }
        
        /* Bot name in message */
        .bot-message-name {
            font-size: 0.75em;
            font-weight: bold;
            color: #007bb5; /* Telegram blue */
            margin-bottom: 2px;
            display: block;
        }

        .message img {
            max-width: 100%;
            height: auto;
            border-radius: 8px;
            margin-top: 5px;
            display: block;
        }

        .message audio {
            width: 100%;
            margin-top: 5px;
            border-radius: 8px;
            background-color: #f0f0f0;
        }
        .bot-message audio {
            background-color: rgba(224, 224, 224, 0.5); /* 50% opacity gray for audio */
        }

        /* Search Highlight Style (မပြောင်းလဲပါ) */
        .highlight {
            background-color: #ffda6a;
            padding: 2px 0;
            border-radius: 3px;
        }
        .highlight.current {
            background-color: #f7a048;
        }


        .chat-input-container {
            display: flex;
            padding: 8px 15px;
            background-color: #f0f0f0;
            border-top: 1px solid #ddd;
            align-items: flex-end; /* Align items to the bottom */
            flex-shrink: 0;
        }

        .input-actions {
            display: flex;
            gap: 5px;
            margin-right: 10px;
        }
        .input-actions button {
            background: none;
            border: none;
            cursor: pointer;
            padding: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 38px;
            height: 38px;
            border-radius: 50%;
            color: #555;
            transition: background-color 0.2s;
        }
        .input-actions button:hover {
            background-color: #e0e0e0;
        }
        .input-actions button .material-icons {
            font-size: 24px;
        }

        #user-input {
            flex-grow: 1;
            padding: 10px 15px;
            border: 1px solid #ccc;
            border-radius: 20px;
            outline: none;
            font-size: 1em;
            background-color: white;
            resize: none;
            min-height: 20px;
            max-height: 100px;
            overflow-y: auto;
            line-height: 1.4;
            padding-top: 12px;
            padding-bottom: 12px;
        }

        #send-button {
            background-color: #0088cc;
            color: white;
            border: none;
            border-radius: 50%;
            width: 40px; 
            height: 40px; 
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 1.5em; 
            cursor: pointer;
            transition: background-color 0.2s;
            flex-shrink: 0;
            margin-bottom: 6px; 
            margin-left: 10px; 
        }
        #send-button .material-icons {
            font-size: inherit;
        }

        #send-button:hover {
            background-color: #007bb5;
        }

        /* Profile Info Modal CSS (REVISED for smoother animation) */
        .profile-modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7); 
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 100;
            opacity: 0;
            visibility: hidden;
            pointer-events: none; /* Prevent clicks when hidden */
            transition: opacity 0.3s ease-in-out, visibility 0.3s ease-in-out; /* For closing animation */
        }

        .profile-modal-overlay.active {
            opacity: 1;
            visibility: visible;
            pointer-events: auto; /* Allow clicks when active */
        }

        .profile-modal {
            background-color: #f7f7f7; 
            border-radius: 15px; 
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.4); 
            width: 90%;
            max-width: 420px; 
            padding: 25px; 
            text-align: center;
            transform: scale(0.9); /* Initial state for animation */
            opacity: 0; 
            /* transition: transform 0.3s ease-out, opacity 0.3s ease-out; */ /* Removed this, using animation for opening */
            position: relative;
            max-height: 90vh; 
            overflow-y: auto; 
            color: #222; 
            border: none; 
        }

        /* Entrance animation for the modal */
        @keyframes fadeInScale {
            from {
                opacity: 0;
                transform: scale(0.9);
            }
            to {
                opacity: 1;
                transform: scale(1);
            }
        }

        .profile-modal-overlay.active .profile-modal {
            animation: fadeInScale 0.3s ease-out forwards; /* Apply animation for opening */
        }
        
        /* For closing, rely on the overlay's transition and modal's initial state if active class removed */
        .profile-modal-overlay:not(.active) .profile-modal {
             /* When overlay is not active, ensure modal goes back to initial hidden state */
            opacity: 0; 
            transform: scale(0.9);
            transition: transform 0.3s ease-out, opacity 0.3s ease-out; /* Add transition back for closing */
        }


        .profile-modal .close-button {
            position: absolute;
            top: 10px; 
            right: 10px; 
            background: none;
            border: none;
            font-size: 30px; 
            color: #aaa; 
            cursor: pointer;
            transition: color 0.2s ease, transform 0.2s ease;
            padding: 5px; 
            border-radius: 50%;
        }
        .profile-modal .close-button:hover {
            color: #666;
            transform: rotate(90deg) scale(1.1); 
            background-color: #eee; 
        }

        .profile-modal .profile-pic-large {
            width: 130px; 
            height: 130px;
            border-radius: 50%;
            overflow: hidden;
            margin: 0 auto 25px; 
            background-color: #e0f2f7; 
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 5em; 
            color: #0088cc;
            border: 5px solid #0088cc; 
            box-shadow: 0 6px 20px rgba(0, 136, 204, 0.3); 
            transition: transform 0.3s ease; 
        }
        .profile-modal .profile-pic-large:hover {
            transform: scale(1.08); 
        }
        .profile-modal .profile-pic-large img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            display: block;
        }

        .profile-modal h2 {
            margin-top: 0;
            color: #0056b3; 
            font-size: 2em; 
            margin-bottom: 12px; 
            font-weight: 600; 
        }

        .profile-modal .status {
            color: #28a745; 
            font-weight: 600;
            font-size: 1.05em; 
            margin-bottom: 30px; 
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px; 
        }
        .profile-modal .status .material-icons {
            font-size: 1.2em; 
        }

        .profile-modal p {
            color: #444;
            line-height: 1.8; 
            margin-bottom: 30px; 
            font-size: 1em; 
        }

        .profile-modal .info-grid {
            display: flex; 
            flex-direction: column; 
            gap: 1px; 
            margin-top: 25px; 
            border-top: 1px solid #eee; 
            border-bottom: 1px solid #eee; 
            background-color: #fff; 
            border-radius: 8px; 
            overflow: hidden; 
        }

        .profile-modal .info-item {
            display: flex;
            align-items: center;
            text-align: left;
            padding: 14px 20px; 
            border-bottom: 1px solid #f0f0f0; 
            color: #555;
            font-size: 1em; 
            transition: background-color 0.2s ease, transform 0.1s ease;
            cursor: pointer; 
        }
        .profile-modal .info-item:last-child {
            border-bottom: none; 
        }
        .profile-modal .info-item:hover {
            background-color: #eef8ff; 
            transform: translateX(5px); 
        }
        .profile-modal .info-item:active {
            background-color: #ddedfa; 
        }

        .profile-modal .info-item .material-icons {
            font-size: 1.4em; 
            margin-right: 15px; 
            color: #0088cc;
            flex-shrink: 0; 
        }
        .profile-modal .info-item a {
            color: #0088cc; 
            text-decoration: none;
            font-weight: 500;
        }
        .profile-modal .info-item a:hover {
            text-decoration: underline;
        }
        .profile-modal .info-item span {
            flex-grow: 1; 
        }

        /* --- Reaction Buttons CSS --- */
        .message-reactions {
            position: absolute;
            bottom: -20px; /* Position below the message bubble */
            /* Adjusted positioning for user/bot messages */
            display: flex;
            align-items: center;
            gap: 2px;
            background-color: rgba(255, 255, 255, 0.9);
            border-radius: 12px;
            padding: 2px 6px;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
            transition: opacity 0.2s ease;
            opacity: 0; /* Hidden by default */
            pointer-events: none; /* No interaction when hidden */
            z-index: 5;
        }
        .message-container:hover .message-reactions {
            opacity: 1; /* Show on message hover */
            pointer-events: auto; /* Allow interaction on hover */
        }

        .message-container.bot-container .message-reactions {
            left: 55px; /* Aligned with bot message */
        }

        .message-container.user-container .message-reactions {
            right: 10px; /* Aligned with user message */
        }
        
        .reaction-button {
            background: none;
            border: none;
            cursor: pointer;
            font-size: 1.1em; /* Icon size */
            padding: 3px 4px;
            border-radius: 10px;
            transition: background-color 0.15s ease, transform 0.1s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #777; /* Default icon color */
        }
        .reaction-button:hover {
            background-color: rgba(0, 0, 0, 0.08);
            transform: scale(1.05);
        }
        .reaction-button.selected {
            background-color: #0088cc;
            color: white;
            transform: scale(1.1);
        }
        .reaction-button.selected:hover {
            background-color: #007bb5;
        }

        .reaction-count {
            font-size: 0.7em;
            color: #666;
            margin-left: 2px;
        }
        .reaction-button.selected .reaction-count {
            color: white;
        }

        /* Displayed reaction summary */
        .displayed-reactions {
            position: absolute;
            /* Position relative to message bubble */
            bottom: 0px; 
            padding: 2px 6px;
            border-radius: 12px;
            background-color: rgba(255, 255, 255, 0.9);
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
            display: flex;
            align-items: center;
            gap: 4px;
            font-size: 0.85em;
            color: #333;
            height: 20px; /* Fixed height to align with reaction buttons */
            transform: translateY(calc(100% + 5px)); /* Position below message, above reactions buttons */
            z-index: 6; /* Higher than reaction buttons */
        }
        .message-container.bot-container .displayed-reactions {
            left: 50px; /* Align with bot message bubble edge */
        }
        .message-container.user-container .displayed-reactions {
            right: 5px; /* Align with user message bubble edge */
        }

        .displayed-reactions .material-icons {
            font-size: 1.1em;
        }
        .displayed-reactions .reaction-icon-count {
            display: flex;
            align-items: center;
            gap: 2px;
        }
        .displayed-reactions .reaction-icon-count span {
            font-size: 0.9em;
        }

    </style>
</head>
<body>
    <div class="chat-container">
        <div class="chat-header" id="chat-header">
            <div class="profile-info-area" id="profile-info-area">
                <div class="profile-pic">
                    <img id="bot-profile-img" src="YOUR_BOT_PROFILE_IMAGE_URL_HERE" alt="Bot Profile Picture" style="display: none;">
                    <i class="material-icons" id="bot-profile-icon">robot</i> 
                </div>
                <div class="info">
                    <h3 id="bot-name-header">ယွန်းရတီဦး</h3>
                    <p>online</p>
                </div>
            </div>
            <div class="icons">
                <i class="material-icons" id="search-icon">search</i> 
                <i class="material-icons" id="more-vert-icon">more_vert</i>
            </div>

            <div class="header-menu" id="header-menu">
                <ul>
                    <li id="clear-history-option"><i class="material-icons">delete_outline</i> ချတ်မှတ်တမ်းရှင်းလင်းမည်</li>
                    <li><i class="material-icons">person_add</i> ဆက်သွယ်မှုထည့်မည်</li>
                 
                    <li><i class="material-icons">notifications_off</i> အသံပိတ်မည်</li></a>
                </ul>
            </div>

            <div class="search-bar" id="search-bar">
                <i class="material-icons" id="search-back-button">arrow_back</i>
                <input type="text" id="search-input" placeholder="ရှာဖွေရန်..." autocomplete="off">
                <span class="search-count" id="search-count">0/0</span>
                <div class="search-nav-buttons">
                    <i class="material-icons" id="search-prev-button">keyboard_arrow_up</i>
                    <i class="material-icons" id="search-next-button">keyboard_arrow_down</i>
                </div>
                <i class="material-icons" id="search-close-button">close</i>
            </div>
        </div>
        <div class="chat-messages" id="chat-messages">
            </div>
        <div class="chat-input-container">
            <div class="input-actions">
                <button id="attach-button" title="ပုံ သို့မဟုတ် ဖိုင်ပူးတွဲရန်"><i class="material-icons">attach_file</i></button>
                <button id="voice-button" title="အသံမက်ဆေ့ချ်ပို့ရန်"><i class="material-icons">mic</i></button>
            </div>
            <textarea id="user-input" placeholder="မက်ဆေ့ချ်ရိုက်ထည့်ပါ..." rows="1"></textarea>
            <button id="send-button"><i class="material-icons">send</i></button>
        </div>
    </div>

    <div class="profile-modal-overlay" id="profile-modal-overlay">
        <div class="profile-modal" id="profile-modal">
            <button class="close-button" id="close-profile-modal">&times;</button>
            <div class="profile-pic-large">
                <img id="bot-profile-img-modal" src="YOUR_BOT_PROFILE_IMAGE_URL_HERE" alt="Bot Profile Picture" style="display: none;">
                <i class="material-icons" id="bot-profile-icon-modal">robot</i> 
            </div>
            <h2 id="bot-name-modal">ယွန်းရတီဦး</h2>
            <p class="status"><i class="material-icons">circle</i> Online</p>
            <p>မင်္ဂလာပါ! ကျွန်မသည် သင့်မေးခွန်းများအား ဖြေကြားရန်နှင့် အချက်အလက်များပေးရန် ဖန်တီးထားသော မိန်းကလေး တစ်ဦးဖြစ်ပါသည်။ ပုံများ၊ အသံဖိုင်များကိုပါ ပြသနိုင်ပါသည်။</p>
            <div class="info-grid">
                <div class="info-item">
                    <i class="material-icons">info_outline</i> <span>**ဗားရှင်း:** 1.0.0</span>
                </div>
                <div class="info-item">
                    <i class="material-icons">developer_mode</i> <span>**ဖန်တီးသူ:** သင့်အမည်/အဖွဲ့အစည်း</span>
                </div>
                <div class="info-item">
                    <i class="material-icons">email</i> <span>**ဆက်သွယ်ရန်:** <a href="mailto:info@example.com">info@example.com</a></span>
                </div>
                <div class="info-item">
                    <i class="material-icons">link</i> <span>**ဝက်ဘ်ဆိုဒ်:** <a href="https://t.me/pumpgptmyanmar" target="_blank">example.com</a></span>
                </div>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const chatMessages = document.getElementById('chat-messages');
            const userInput = document.getElementById('user-input');
            const sendButton = document.getElementById('send-button');
            const attachButton = document.getElementById('attach-button');
            const voiceButton = document.getElementById('voice-button');
            
            const chatHeader = document.getElementById('chat-header');
            const profileInfoArea = document.getElementById('profile-info-area'); // New: Specific area for profile click
            const moreVertIcon = document.getElementById('more-vert-icon');
            const headerMenu = document.getElementById('header-menu');
            const clearHistoryOption = document.getElementById('clear-history-option');

            const searchIcon = document.getElementById('search-icon');
            const searchBar = document.getElementById('search-bar');
            const searchInput = document.getElementById('search-input');
            const searchCount = document.getElementById('search-count');
            const searchPrevButton = document.getElementById('search-prev-button');
            const searchNextButton = document.getElementById('search-next-button');
            const searchBackButton = document.getElementById('search-back-button');
            const searchCloseButton = document.getElementById('search-close-button');

            const botProfileImg = document.getElementById('bot-profile-img');
            const botProfileIcon = document.getElementById('bot-profile-icon');
            const botNameHeader = document.getElementById('bot-name-header');

            const profileModalOverlay = document.getElementById('profile-modal-overlay');
            const profileModal = document.getElementById('profile-modal');
            const closeProfileModalButton = document.getElementById('close-profile-modal');
            const botProfileImgModal = document.getElementById('bot-profile-img-modal'); 
            const botProfileIconModal = document.getElementById('bot-profile-icon-modal'); 
            const botNameModal = document.getElementById('bot-name-modal');


            const API_URL = 'https://opensheet.elk.sh/1KUPfOqKwNkt_rMTEzvBOwctKNEjti9oqUEAHz6CVkqM/1'; 

            let lastBotResponse = {};
            let searchResults = []; 
            let currentSearchIndex = -1; 

            // **YOUR BOT'S PROFILE INFORMATION**
            const botInfo = {
                name: "ယွန်းရတီဦး", // Bot ရဲ့ နာမည်
                
                
                profileImageUrl: "https://i.pinimg.com/736x/77/4c/47/774c476504b9199dbcdb7d7857c217db.jpg", // Bot ရဲ့ Profile ပုံ Link
                description: "မင်္ဂလာပါ! ကျွန်မသည် သင့်မေးခွန်းများအား ဖြေကြားရန်နှင့် အချက်အလက်များပေးရန် ဖန်တီးထားသော မိန်းကလေး တစ်ဦး ဖြစ်ပါသည်။ ပုံများ၊ အသံဖိုင်များကိုပါ ပြသနိုင်ပါသည်။",
                version: "1.0.0",
                creator: "ပန့်သကို",
                contactEmail: "noblenender@gmail.com",
                website: "https://t.me/pumpgptmyanmar"
            };

            // Define reactions with their icons and colors
            const reactions = [
                { id: 'like', icon: 'thumb_up', color: '#0088cc' },
                { id: 'love', icon: 'favorite', color: '#e0245e' },
                { id: 'haha', icon: 'sentiment_very_satisfied', color: '#ffc107' },
                { id: 'wow', icon: 'sentiment_neutral', color: '#9c27b0' }, // Changed to sentiment_neutral for 'wow'
                { id: 'sad', icon: 'sentiment_dissatisfied', color: '#607d8b' },
                { id: 'angry', icon: 'sentiment_very_dissatisfied', color: '#dc3545' }
            ];

            function addMessage(content, type, messageType) {
                const messageContainer = document.createElement('div');
                messageContainer.classList.add('message-container', `${type}-container`);
                messageContainer.dataset.messageId = Date.now() + Math.random().toString(36).substring(2, 9); // Unique ID

                const messageDiv = document.createElement('div');
                messageDiv.classList.add('message', `${type}-message`);
                
                // Add profile picture for bot messages
                if (type === 'bot') {
                    const profilePicDiv = document.createElement('div');
                    profilePicDiv.classList.add('message-profile-pic');
                    if (botProfileImg.src && botProfileImg.style.display !== 'none') {
                        const img = document.createElement('img');
                        img.src = botProfileImg.src;
                        profilePicDiv.appendChild(img);
                    } else {
                        const icon = document.createElement('i');
                        icon.classList.add('material-icons');
                        icon.textContent = 'robot'; // Default icon
                        profilePicDiv.appendChild(icon);
                    }
                    messageContainer.appendChild(profilePicDiv);

                    // Add bot name for bot messages
                    const botNameSpan = document.createElement('span');
                    botNameSpan.classList.add('bot-message-name');
                    botNameSpan.textContent = botInfo.name;
                    messageDiv.appendChild(botNameSpan);
                }

                if (content.text && content.text.trim() !== '') {
                    const pTag = document.createElement('p');
                    const formattedText = content.text.replace(/(https?:\/\/[^\s]+)/g, '<a href="$1" target="_blank" style="color: inherit; text-decoration: underline;">$1</a>');
                    pTag.innerHTML = formattedText;
                    messageDiv.appendChild(pTag);
                }

                if (content.image && content.image.trim() !== '') {
                    const imgElement = document.createElement('img');
                    imgElement.src = content.image;
                    imgElement.alt = "Image from bot";
                    messageDiv.appendChild(imgElement);
                }

                if (content.voice && content.voice.trim() !== '') {
                    const audioElement = document.createElement('audio');
                    audioElement.controls = true;
                    audioElement.src = content.voice;
                    messageDiv.appendChild(audioElement);
                }
                
                messageContainer.appendChild(messageDiv);
                chatMessages.appendChild(messageContainer);

                // Add Reaction Buttons
                addReactionButtons(messageContainer, type);
                // Add Displayed Reactions area
                addDisplayedReactions(messageContainer);

                chatMessages.scrollTop = chatMessages.scrollHeight;
            }

            // --- Reaction Feature Functions ---
            function addReactionButtons(messageContainer, type) {
                const reactionsDiv = document.createElement('div');
                reactionsDiv.classList.add('message-reactions');

                const messageId = messageContainer.dataset.messageId;

                const currentReactions = JSON.parse(localStorage.getItem(`reactions_${messageId}`)) || {};
                
                if (type === 'bot') {
                    reactions.forEach(reaction => {
                        const button = document.createElement('button');
                        button.classList.add('reaction-button');
                        button.dataset.reactionType = reaction.id;
                        button.innerHTML = `<i class="material-icons">${reaction.icon}</i>`;
                        button.style.color = reaction.color; 
                        
                        // Check if this reaction was previously selected by user
                        if (currentReactions[reaction.id] && currentReactions[reaction.id].includes('user')) {
                            button.classList.add('selected');
                        }

                        button.addEventListener('click', () => toggleReaction(messageId, reaction.id, type));
                        reactionsDiv.appendChild(button);
                    });
                } else { // For user messages, only a "like" button
                    const likeReaction = reactions.find(r => r.id === 'like');
                    if (likeReaction) {
                        const button = document.createElement('button');
                        button.classList.add('reaction-button');
                        button.dataset.reactionType = likeReaction.id;
                        button.innerHTML = `<i class="material-icons">${likeReaction.icon}</i>`;
                        button.style.color = likeReaction.color;

                        if (currentReactions[likeReaction.id] && currentReactions[likeReaction.id].includes('user')) {
                            button.classList.add('selected');
                        }

                        button.addEventListener('click', () => toggleReaction(messageId, likeReaction.id, type));
                        reactionsDiv.appendChild(button);
                    }
                }
                messageContainer.appendChild(reactionsDiv);
            }

            function addDisplayedReactions(messageContainer) {
                const displayedReactionsDiv = document.createElement('div');
                displayedReactionsDiv.classList.add('displayed-reactions');
                messageContainer.appendChild(displayedReactionsDiv);
                updateDisplayedReactions(messageContainer); // Initial update
            }

            function toggleReaction(messageId, reactionType, messageOwnerType) {
                let reactionsData = JSON.parse(localStorage.getItem(`reactions_${messageId}`)) || {};

                // Initialize reaction type if it doesn't exist
                if (!reactionsData[reactionType]) {
                    reactionsData[reactionType] = [];
                }

                const userIndex = reactionsData[reactionType].indexOf('user');

                if (userIndex > -1) {
                    // User already reacted with this, so remove it
                    reactionsData[reactionType].splice(userIndex, 1);
                    if (reactionsData[reactionType].length === 0) {
                        delete reactionsData[reactionType]; // Remove if no one is left
                    }
                } else {
                    // Remove any previous reaction by user before adding new one
                    for (const key in reactionsData) {
                        const index = reactionsData[key].indexOf('user');
                        if (index > -1) {
                            reactionsData[key].splice(index, 1);
                            if (reactionsData[key].length === 0) {
                                delete reactionsData[key];
                            }
                        }
                    }
                    // Add the new reaction
                    reactionsData[reactionType].push('user');
                }

                localStorage.setItem(`reactions_${messageId}`, JSON.stringify(reactionsData));
                
                // Update button state visually
                const messageContainer = document.querySelector(`[data-message-id="${messageId}"]`);
                if (messageContainer) {
                    const reactionButtons = messageContainer.querySelectorAll('.reaction-button');
                    reactionButtons.forEach(button => {
                        if (button.dataset.reactionType === reactionType) {
                            if (userIndex === -1) { // Was not selected, now selected
                                button.classList.add('selected');
                            } else { // Was selected, now deselected
                                button.classList.remove('selected');
                            }
                        } else { // Deselect other buttons
                            button.classList.remove('selected');
                        }
                    });
                    updateDisplayedReactions(messageContainer);
                }
            }

            function updateDisplayedReactions(messageContainer) {
                const messageId = messageContainer.dataset.messageId;
                const reactionsData = JSON.parse(localStorage.getItem(`reactions_${messageId}`)) || {};
                const displayedReactionsDiv = messageContainer.querySelector('.displayed-reactions');
                displayedReactionsDiv.innerHTML = ''; // Clear existing

                const totalReactions = Object.values(reactionsData).flat().length;

                if (totalReactions === 0) {
                    displayedReactionsDiv.style.display = 'none';
                    return;
                }
                displayedReactionsDiv.style.display = 'flex'; // Show if reactions exist

                // Sort reactions by count (descending) then by ID for consistency
                const sortedReactionTypes = Object.keys(reactionsData).sort((a, b) => {
                    const countA = reactionsData[a].length;
                    const countB = reactionsData[b].length;
                    if (countA !== countB) return countB - countA;
                    return a.localeCompare(b);
                });

                sortedReactionTypes.forEach(type => {
                    if (reactionsData[type].length > 0) {
                        const reactionInfo = reactions.find(r => r.id === type);
                        if (reactionInfo) {
                            const reactionIconCountDiv = document.createElement('div');
                            reactionIconCountDiv.classList.add('reaction-icon-count');
                            reactionIconCountDiv.innerHTML = `
                                <i class="material-icons" style="color: ${reactionInfo.color};">${reactionInfo.icon}</i>
                                <span>${reactionsData[type].length}</span>
                            `;
                            displayedReactionsDiv.appendChild(reactionIconCountDiv);
                        }
                    }
                });
            }

            function loadMessagesWithReactions() {
                chatMessages.innerHTML = ''; // Clear current messages
                const savedMessages = JSON.parse(localStorage.getItem('chatMessages')) || [];

                if (savedMessages.length === 0) {
                    addInitialBotMessage();
                } else {
                    savedMessages.forEach(msg => {
                        addMessage(msg.content, msg.type, msg.messageType);
                    });
                }
            }

            function saveMessage(content, type, messageType) {
                const savedMessages = JSON.parse(localStorage.getItem('chatMessages')) || [];
                savedMessages.push({ content, type, messageType });
                localStorage.setItem('chatMessages', JSON.stringify(savedMessages));
            }


            function addInitialBotMessage() {
                const initialMessageContent = {
                    text: "မင်္ဂလာပါ! ကျွန်မက သင်ပို့တဲ့စာပေါ်မူတည်ပြီး အချက်အလက်တွေနဲ့ ပုံတွေကိုပါ ပြသပေးနိုင်ပါတယ်။"
                };
                addMessage(initialMessageContent, 'bot', 'text');
                saveMessage(initialMessageContent, 'bot', 'text');
            }

            async function sendMessage(userMessageType = 'text', userContent = null) {
                let displayedUserContent = '';

                if (userMessageType === 'text') {
                    displayedUserContent = userInput.value.trim();
                    if (displayedUserContent === '') return;
                    addMessage({text: displayedUserContent}, 'user', 'text');
                    saveMessage({text: displayedUserContent}, 'user', 'text'); // Save user message
                } else if (userMessageType === 'image') {
                    if (userContent) addMessage({image: userContent}, 'user', 'image');
                    displayedUserContent = "ပုံတစ်ပုံ ပို့လိုက်ပါပြီ။";
                    if (userContent) saveMessage({image: userContent}, 'user', 'image'); // Save user image message
                } else if (userMessageType === 'voice') {
                    if (userContent) addMessage({voice: userContent}, 'user', 'voice');
                    displayedUserContent = "အသံမက်ဆေ့ချ် ပို့လိုက်ပါပြီ။";
                    if (userContent) saveMessage({voice: userContent}, 'user', 'voice'); // Save user voice message
                }

                userInput.value = '';
                userInput.style.height = 'auto';

                const typingMessageDiv = document.createElement('div');
                typingMessageDiv.classList.add('message-container', 'bot-container', 'typing-indicator');
                typingMessageDiv.innerHTML = `
                    <div class="message-profile-pic">
                        ${botProfileImg.src && botProfileImg.style.display !== 'none' ? `<img src="${botProfileImg.src}" alt="Bot Profile Picture">` : '<i class="material-icons">robot</i>'}
                    </div>
                    <div class="message bot-message">
                        <span class="bot-message-name">${botInfo.name}</span>
                        <p>Bot သည် စာရိုက်နေပါသည်...</p>
                    </div>
                `;
                chatMessages.appendChild(typingMessageDiv);
                chatMessages.scrollTop = chatMessages.scrollHeight;

                try {
                    const response = await fetch(API_URL);
                    const data = await response.json();

                    let botResponseText = "တောင်းပန်ပါတယ်၊ နားမလည်ပါဘူး။ တခြားမေးခွန်းတစ်ခု မေးကြည့်ပါ။";
                    let botResponseImage = null;
                    let botResponseVoice = null;
                    let botReplyType = 'text';
                    
                    const cleanedUserMessage = displayedUserContent.toLowerCase().trim(); 
                    let foundMatch = false; 

                    if (Array.isArray(data) && data.length > 0) {
                        const matchingResponses = []; 
                        
                        for (const row of data) {
                            const cleanedTrigger = row.Trigger ? row.Trigger.toLowerCase().trim() : '';
                            const userWords = cleanedUserMessage.split(/\s+/);
                            const triggerWords = cleanedTrigger.split(/\s+/);

                            const containsAllTriggerWords = triggerWords.every(triggerWord => userWords.includes(triggerWord));

                            if (cleanedTrigger && containsAllTriggerWords) {
                                matchingResponses.push({
                                    reply_content: row.reply_content,
                                    reply_type: row.reply_type,
                                    original_trigger: row.Trigger
                                });
                            }
                        }

                        if (matchingResponses.length > 0) {
                            foundMatch = true;
                            let chosenResponse = null;

                            const lastContent = lastBotResponse[cleanedUserMessage];

                            if (lastContent) {
                                const otherResponses = matchingResponses.filter(
                                    res => res.reply_content !== lastContent
                                );
                                if (otherResponses.length > 0) {
                                    chosenResponse = otherResponses[Math.floor(Math.random() * otherResponses.length)];
                                } else {
                                    chosenResponse = matchingResponses[0];
                                }
                            } else {
                                chosenResponse = matchingResponses[Math.floor(Math.random() * matchingResponses.length)];
                            }

                            if (chosenResponse) {
                                lastBotResponse[cleanedUserMessage] = chosenResponse.reply_content;

                                botReplyType = chosenResponse.reply_type;
                                const contentParts = chosenResponse.reply_content ? chosenResponse.reply_content.split('||') : [];
                                
                                switch(botReplyType) {
                                    case 'text':
                                        botResponseText = contentParts[0] ? contentParts[0].trim() : "အဖြေမတွေ့ပါ။";
                                        break;
                                    case 'image':
                                        botResponseImage = contentParts[0] ? contentParts[0].trim() : null;
                                        botResponseText = ""; 
                                        break;
                                    case 'voice':
                                        botResponseVoice = contentParts[0] ? contentParts[0].trim() : null;
                                        botResponseText = ""; 
                                        break;
                                    case 'text_and_image':
                                        botResponseText = contentParts[0] ? contentParts[0].trim() : "အဖြေမတွေ့ပါ။";
                                        botResponseImage = contentParts[1] ? contentParts[1].trim() : null;
                                        break;
                                    case 'text_and_voice':
                                        botResponseText = contentParts[0] ? contentParts[0].trim() : "အဖြေမတွေ့ပါ။";
                                        botResponseVoice = contentParts[1] ? contentParts[1].trim() : null;
                                        break;
                                    default:
                                        botResponseText = "ပြန်ဖြေနည်း အမျိုးအစားကို နားမလည်ပါ။";
                                }
                            }
                        }
                    }

                    if (!foundMatch) {
                        botResponseText = "တောင်းပန်ပါတယ်၊ နားမလည်ပါဘူး။ တခြားမေးခွန်းတစ်ခု မေးကြည့်ပါ။";
                        botResponseImage = null;
                        botResponseVoice = null;
                        botReplyType = 'text';
                    }

                    if (typingMessageDiv) {
                        chatMessages.removeChild(typingMessageDiv);
                    }

                    const botMessageContent = { text: botResponseText, image: botResponseImage, voice: botResponseVoice };
                    addMessage(botMessageContent, 'bot', botReplyType);
                    saveMessage(botMessageContent, 'bot', botReplyType); // Save bot message

                } catch (error) {
                    console.error('အချက်အလက်ရယူရာတွင် အမှားဖြစ်သည်:', error);
                    if (typingMessageDiv) {
                        chatMessages.removeChild(typingMessageDiv);
                    }
                    const errorMessageContent = { text: "အာ! အချက်အလက်ရယူရာတွင် တစ်ခုခု မှားယွင်းသွားပါသည်။ နောက်မှ ထပ်ကြိုးစားကြည့်ပါ။" };
                    addMessage(errorMessageContent, 'bot', 'text');
                    saveMessage(errorMessageContent, 'bot', 'text'); // Save error message
                }
            }

            // --- Search Functionality (No Changes) ---
            function highlightSearchTerm(term) {
                removeAllHighlights();

                if (!term) {
                    searchResults = [];
                    currentSearchIndex = -1;
                    updateSearchCount();
                    return;
                }

                searchResults = [];
                currentSearchIndex = -1;

                const messages = chatMessages.querySelectorAll('.message p'); 
                const regex = new RegExp(`(${term})`, 'gi'); 

                messages.forEach(pTag => {
                    const originalText = pTag.innerHTML;
                    let replacedText = originalText.replace(regex, (match) => {
                        const span = document.createElement('span');
                        span.classList.add('highlight');
                        span.textContent = match;
                        searchResults.push(span); 
                        return span.outerHTML; 
                    });
                    pTag.innerHTML = replacedText;
                });

                updateSearchCount();
                if (searchResults.length > 0) {
                    currentSearchIndex = 0; 
                    scrollToCurrentResult();
                }
            }

            function removeAllHighlights() {
                chatMessages.querySelectorAll('.highlight').forEach(span => {
                    const parent = span.parentNode;
                    parent.replaceChild(document.createTextNode(span.textContent), span);
                    parent.normalize(); 
                });
                searchResults = [];
                currentSearchIndex = -1;
                updateSearchCount();
            }

            function updateSearchCount() {
                if (searchResults.length > 0) {
                    searchCount.textContent = `${currentSearchIndex + 1}/${searchResults.length}`;
                } else {
                    searchCount.textContent = `0/0`;
                }
            }

            function scrollToCurrentResult() {
                removeAllHighlightClasses(); 
                if (currentSearchIndex >= 0 && currentSearchIndex < searchResults.length) {
                    const currentSpan = searchResults[currentSearchIndex];
                    currentSpan.classList.add('current'); 
                    currentSpan.scrollIntoView({ behavior: 'smooth', block: 'center' });
                }
            }

            function removeAllHighlightClasses() {
                chatMessages.querySelectorAll('.highlight.current').forEach(span => {
                    span.classList.remove('current');
                });
            }


            // --- Event Listeners ---
            sendButton.addEventListener('click', () => sendMessage('text'));

            userInput.addEventListener('keydown', (e) => {
                if (e.key === 'Enter' && !e.shiftKey) {
                    e.preventDefault();
                    sendMessage('text');
                }
            });

            userInput.addEventListener('input', () => {
                userInput.style.height = 'auto';
                userInput.style.height = userInput.scrollHeight + 'px';
            });

            const fileInput = document.createElement('input');
            fileInput.type = 'file';
            fileInput.accept = 'image/*';
            fileInput.style.display = 'none';
            document.body.appendChild(fileInput);

            attachButton.addEventListener('click', () => {
                fileInput.click();
            });

            fileInput.addEventListener('change', (e) => {
                const file = e.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = (event) => {
                        sendMessage('image', event.target.result); 
                    };
                    reader.readAsDataURL(file);
                }
            });

            voiceButton.addEventListener('click', () => {
                // For demonstration, send a dummy voice message URL
                // In a real app, you'd record and upload here.
                const dummyVoiceUrl = 'https://www.soundhelix.com/examples/mp3/SoundHelix-Song-8.mp3';
                addMessage({text: "အသံမက်ဆေ့ချ် ပို့လိုက်ပါပြီ။ (Dummy)", voice: dummyVoiceUrl}, 'user', 'voice');
                saveMessage({text: "အသံမက်ဆေ့ချ် ပို့လိုက်ပါပြီ။ (Dummy)", voice: dummyVoiceUrl}, 'user', 'voice');
                // You might still send a text message to the bot to trigger a response
                sendMessage('text', "အသံမက်ဆေ့ချ်"); 
            });

            // --- More_vert Menu Logic ---
            moreVertIcon.addEventListener('click', (event) => {
                event.stopPropagation(); // Prevent event from bubbling up to profile-info-area
                headerMenu.classList.toggle('active'); 
                if (searchBar.classList.contains('active')) {
                    searchBar.classList.remove('active');
                    removeAllHighlights(); 
                    searchInput.value = ''; 
                }
                if (profileModalOverlay.classList.contains('active')) { 
                    profileModalOverlay.classList.remove('active');
                }
            });

            document.addEventListener('click', (event) => {
                // Close header menu if clicked outside
                if (!headerMenu.contains(event.target) && headerMenu.classList.contains('active')) {
                    headerMenu.classList.remove('active');
                }
                // Close search bar if clicked outside (but not on search icon)
                if (!searchBar.contains(event.target) && !searchIcon.contains(event.target) && searchBar.classList.contains('active')) {
                    searchBar.classList.remove('active');
                    removeAllHighlights();
                    searchInput.value = '';
                }
                // Close profile modal if clicked outside (but not on profile info area)
                if (profileModalOverlay.classList.contains('active') && !profileModal.contains(event.target) && !profileInfoArea.contains(event.target)) {
                    profileModalOverlay.classList.remove('active');
                }
            });

            clearHistoryOption.addEventListener('click', () => {
                chatMessages.innerHTML = ''; 
                localStorage.removeItem('chatMessages'); // Clear saved messages
                localStorage.clear(); // Clear all reactions as well (simple approach)
                addInitialBotMessage(); 
                headerMenu.classList.remove('active'); 
                removeAllHighlights(); 
            });

            // --- Search Bar Event Listeners ---
            searchIcon.addEventListener('click', (event) => {
                event.stopPropagation(); // Prevent event from bubbling up to profile-info-area
                searchBar.classList.toggle('active'); 
                if (headerMenu.classList.contains('active')) {
                    headerMenu.classList.remove('active');
                }
                if (profileModalOverlay.classList.contains('active')) { 
                    profileModalOverlay.classList.remove('active');
                }
                if (searchBar.classList.contains('active')) {
                    searchInput.focus(); 
                } else {
                    removeAllHighlights(); 
                    searchInput.value = ''; 
                }
            });

            searchBackButton.addEventListener('click', () => {
                searchBar.classList.remove('active');
                removeAllHighlights();
                searchInput.value = '';
            });

            searchCloseButton.addEventListener('click', () => {
                searchBar.classList.remove('active');
                removeAllHighlights();
                searchInput.value = '';
            });

            searchInput.addEventListener('input', () => {
                highlightSearchTerm(searchInput.value);
            });

            searchInput.addEventListener('keydown', (e) => {
                if (e.key === 'Enter') {
                    if (searchResults.length > 0) {
                        currentSearchIndex = (currentSearchIndex + 1) % searchResults.length;
                        scrollToCurrentResult();
                    }
                    e.preventDefault(); 
                }
            });

            searchNextButton.addEventListener('click', () => {
                if (searchResults.length > 0) {
                    currentSearchIndex = (currentSearchIndex + 1) % searchResults.length;
                    scrollToCurrentResult();
                }
            });

            searchPrevButton.addEventListener('click', () => {
                if (searchResults.length > 0) {
                    currentSearchIndex = (currentSearchIndex - 1 + searchResults.length) % searchResults.length;
                    scrollToCurrentResult();
                }
            });

            // --- Bot Profile Image & Name Initialization ---
            const initializeBotProfile = () => {
                // Set Header Bot Name
                botNameHeader.textContent = botInfo.name;

                // Set Modal Bot Name
                botNameModal.textContent = botInfo.name;

                // Set Description, Version, Creator, Contact, Website in Modal
                profileModal.querySelector('p:not(.status)').textContent = botInfo.description;
                profileModal.querySelector('.info-item:nth-child(1) span').innerHTML = `**ဗားရှင်း:** ${botInfo.version}`;
                profileModal.querySelector('.info-item:nth-child(2) span').innerHTML = `**ဖန်တီးသူ:** ${botInfo.creator}`;
                
                const emailLink = profileModal.querySelector('.info-item:nth-child(3) a');
                emailLink.href = `mailto:${botInfo.contactEmail}`;
                emailLink.textContent = botInfo.contactEmail;

                const websiteLink = profileModal.querySelector('.info-item:nth-child(4) a');
                websiteLink.href = botInfo.website;
                websiteLink.textContent = botInfo.website.replace(/(^\w+:|^)\/\//, ''); // Remove http/https for display

                // Load Profile Image
                const img = new Image();
                img.onload = () => {
                    botProfileImg.src = botInfo.profileImageUrl;
                    botProfileImg.style.display = 'block';
                    botProfileIcon.style.display = 'none';

                    botProfileImgModal.src = botInfo.profileImageUrl;
                    botProfileImgModal.style.display = 'block';
                    botProfileIconModal.style.display = 'none';
                    
                    loadMessagesWithReactions(); // Load messages after profile is initialized
                };
                img.onerror = () => {
                    console.warn('Bot profile image failed to load, displaying default icon.');
                    botProfileImg.style.display = 'none';
                    botProfileIcon.style.display = 'flex';

                    botProfileImgModal.style.display = 'none';
                    botProfileIconModal.style.display = 'flex';

                    loadMessagesWithReactions(); // Load messages even if image fails
                };
                img.src = botInfo.profileImageUrl;
            };

            initializeBotProfile(); 

            // --- Profile Modal Logic (Revised for smoother animation) ---
            profileInfoArea.addEventListener('click', (event) => {
                event.stopPropagation(); 
                
                if (searchBar.classList.contains('active')) {
                    searchBar.classList.remove('active');
                    removeAllHighlights();
                    searchInput.value = '';
                }
                if (headerMenu.classList.contains('active')) {
                    headerMenu.classList.remove('active');
                }
                // Add active class to overlay to start its transition and trigger modal animation
                profileModalOverlay.classList.add('active'); 
            });

            closeProfileModalButton.addEventListener('click', () => {
                // Remove active class from overlay to start its transition and modal's closing transition
                profileModalOverlay.classList.remove('active'); 
            });

            profileModalOverlay.addEventListener('click', (event) => {
                if (event.target === profileModalOverlay) {
                    profileModalOverlay.classList.remove('active');
                }
            });
        });
    </script>
</body>
</html>
