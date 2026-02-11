<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LoL Universe | Complete & Fixed Edition</title>
    <link href="https://fonts.googleapis.com/css2?family=Prompt:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #c89b3c; --bg-dark: #020617; --card-bg: #0f172a; --current-accent: var(--gold); }
        * { box-sizing: border-box; scroll-behavior: smooth; }
        body { margin: 0; font-family: 'Prompt', sans-serif; background: #000; color: #f1f5f9; min-height: 100vh; overflow-x: hidden; transition: background 0.6s ease-in-out; }
        #fxOverlay { position: fixed; inset: 0; opacity: 0; pointer-events: none; z-index: 9999; transition: 0.5s; }
        .fx-anim { animation: fxBurst 1.2s ease-out forwards; }
        @keyframes fxBurst { 0% { opacity: 0; transform: scale(1); } 30% { opacity: 1; transform: scale(1.1); } 100% { opacity: 0; transform: scale(1.5); } }
        .container { width: 95%; max-width: 1200px; margin: auto; padding: 40px 0; text-align: center; }
        .view-section { display: none; animation: fadeIn 0.4s ease-out; }
        .view-section.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .grid-letters { display: grid; grid-template-columns: repeat(auto-fill, minmax(65px, 1fr)); gap: 10px; }
        .letter-node { height: 65px; display: flex; justify-content: center; align-items: center; font-size: 24px; border-radius: 8px; cursor: pointer; background: #111827; border: 1px solid #1e293b; transition: 0.2s; color: #fff; }
        .letter-node:hover { border-color: var(--gold); color: var(--gold); background: #1e293b; }
        .champ-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(180px, 1fr)); gap: 30px; margin-top: 30px; }
        .champ-card { cursor: pointer; transition: 0.3s; position: relative; border-radius: 15px; overflow: hidden; background: var(--card-bg); border: 1px solid #1e293b; }
        .champ-card img { width: 100%; transition: 0.5s; display: block; }
        .champ-card:hover { border-color: var(--gold); transform: translateY(-5px); }
        .champ-card .name-tag { padding: 15px; font-weight: 700; letter-spacing: 2px; background: var(--card-bg); color: #fff; }
        .btn { padding: 12px 24px; border-radius: 4px; background: rgba(0,0,0,0.7); border: 1px solid var(--current-accent); color: #fff; font-weight: 700; cursor: pointer; transition: 0.3s; text-transform: uppercase; letter-spacing: 1px; }
        .btn:hover:not(:disabled) { background: var(--current-accent); color: #000; box-shadow: 0 0 15px var(--current-accent); }
        .btn.active-skin { background: var(--current-accent); color: #000; font-weight: 800; }
        .btn:disabled { opacity: 0.3; cursor: not-allowed; }
        .skill-item { background: rgba(15, 23, 42, 0.85); border-radius: 12px; padding: 25px; margin-bottom: 30px; border-left: 4px solid var(--current-accent); display: grid; grid-template-columns: 1.2fr 1.8fr; gap: 40px; align-items: center; text-align: left; backdrop-filter: blur(10px); }
        .skill-icon-img { width: 64px; height: 64px; border: 2px solid var(--current-accent); border-radius: 4px; background: #000; }
        .skill-video-box { border-radius: 8px; overflow: hidden; border: 1px solid #334155; aspect-ratio: 16/9; background: #000; }
        .skill-video-box video { width: 100%; height: 100%; object-fit: cover; }
        .back-nav { color: var(--gold); cursor: pointer; margin-bottom: 20px; display: inline-block; font-weight: 600; text-decoration: none; border-bottom: 1px solid transparent; transition: 0.3s; }
        .back-nav:hover { border-bottom-color: var(--gold); }
    </style>
</head>
<body>
<div id="fxOverlay"></div>
<div class="container">
    <div id="index-view" class="view-section active">
        <h1 style="color:var(--gold); letter-spacing: 6px; font-size: 40px; margin-bottom: 40px;">CHAMPION INDEX</h1>
        <div class="grid-letters" id="letters-grid"></div>
    </div>
    <div id="list-view" class="view-section">
        <div class="back-nav" onclick="showView('index-view')">← BACK TO INDEX</div>
        <div class="champ-grid" id="champions-grid"></div>
    </div>
    <div id="profile-view" class="view-section">
        <div class="back-nav" onclick="showView('list-view')">← BACK TO LIST</div>
        <h1 id="prof-name">NAME</h1>
        <p id="prof-title" style="letter-spacing: 10px; color: #94a3b8; margin-bottom: 30px;">TITLE</p>
        <div id="skin-btns" style="margin-bottom: 30px; display: flex; justify-content: center; gap: 10px; flex-wrap: wrap;"></div>
        <div style="margin-bottom: 50px;">
            <button class="btn" id="lockBtn" onclick="handleLock()">LOCK IN</button>
            <button class="btn" id="viewBtn" disabled onclick="handleViewData()" style="margin-left: 10px;">SHOW ABILITIES</button>
        </div>
        <div id="profile-content" style="display:none;">
            <div style="display: grid; grid-template-columns: 1fr 1.6fr; gap: 40px; text-align: left; margin-bottom: 60px;">
                <img id="mainImg" src="" style="width:100%; border-radius:4px; border: 1px solid rgba(255,255,255,0.1);">
                <div>
                    <h2 style="color:var(--current-accent);">BIOGRAPHY</h2>
                    <p id="prof-lore" style="line-height: 1.8; color: #cbd5e1;"></p>
                    <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; border-top: 1px solid #334155; padding-top: 20px;">
                        <p><b>ROLE:</b> <span id="prof-role" style="color:var(--gold)"></span></p>
                        <p><b>REGION:</b> <span id="prof-origin" style="color:var(--gold)"></span></p>
                    </div>
                </div>
            </div>
            <div id="skills-list"></div>
        </div>
    </div>
</div>

<script>
const PATCH_VER = "14.24.1"; 

const champs = {
    "Aatrox": {
        name: "Aatrox", title: "The Darkin Blade", accent: "#dc2626", id: "0266", role: "Fighter", origin: "Shurima / Darkin",
        lore: "ครั้งหนึ่งเขาเคยเป็นวีรบุรุษผู้ต่อสู้เพื่อ Shurima บัดนี้เขาตื่นขึ้นมาเพื่อทำลายโลกเพื่อยุติความทรมานนิรันดร์",
        skins: ["Aatrox_0.jpg", "Aatrox_1.jpg", "Aatrox_2.jpg"],
        skills: [
            { k: "P", n: "Deathbringer Stance", d: "ตีแรงขึ้นตามเลือดสูงสุด", i: "Aatrox_Passive.png" },
            { k: "Q", n: "The Darkin Blade", d: "ฟาดดาบ 3 ครั้ง", i: "AatroxQ.png" },
            { k: "W", n: "Infernal Chains", d: "ฟาดโซ่ดึงศัตรู", i: "AatroxW.png" },
            { k: "E", n: "Umbral Dash", d: "พุ่งตัวและดูดเลือด", i: "AatroxE.png" },
            { k: "R", n: "World Ender", d: "ร่างปีศาจขยายร่าง", i: "AatroxR.png" }
        ]
    },
    "Ahri": {
        name: "Ahri", title: "The Nine-Tailed Fox", accent: "#3b82f6", id: "0103", role: "Mage", origin: "Ionia",
        lore: "จิ้งจอกเก้าหางผู้เดินทางเพื่อค้นหาตัวตนที่แท้จริงและควบคุมพลังวิญญาณ",
        skins: ["Ahri_0.jpg", "Ahri_7.jpg", "Ahri_27.jpg"],
        skills: [
            { k: "P", n: "Essence Theft", d: "สะสมวิญญาณเพื่อฮีล", i: "Ahri_SoulEater2.png" },
            { k: "Q", n: "Orb of Deception", d: "ปาลูกแก้วสร้างดาเมจเวทและจริง", i: "AhriOrbofDeception.png" },
            { k: "W", n: "Fox-Fire", d: "เรียกไฟจิ้งจอกพุ่งใส่", i: "AhriFoxFire.png" },
            { k: "E", n: "Charm", d: "ส่งจูบให้ศัตรูหลงเสน่ห์", i: "AhriSeduce.png" },
            { k: "R", n: "Spirit Rush", d: "พุ่งตัวรวดเร็ว 3 จังหวะ", i: "AhriTumble.png" }
        ]
    },
    "Akali": {
        name: "Akali", title: "The Rogue Assassin", accent: "#10b981", id: "0084", role: "Assassin", origin: "Ionia",
        lore: "นินจาสาวผู้ละทิ้งภาคีเพื่อปกป้อง Ionia ในแนวทางของตัวเอง",
        skins: ["Akali_0.jpg", "Akali_9.jpg", "Akali_32.jpg"],
        skills: [
            { k: "P", n: "Assassin's Mark", d: "สร้างวงแหวนเสริมพลังตี", i: "Akali_Passive.png" },
            { k: "Q", n: "Five Point Strike", d: "ปามีดคุไน 5 เล่ม", i: "AkaliQ.png" },
            { k: "W", n: "Twilight Shroud", d: "ระเบิดควันล่องหน", i: "AkaliW.png" },
            { k: "E", n: "Shuriken Flip", d: "ปาดาวกระจายและพุ่งกลับ", i: "AkaliE.png" },
            { k: "R", n: "Perfect Execution", d: "พุ่งสังหารต่อเนื่อง", i: "AkaliR.png" }
        ]
    },
    "Akshan": {
        name: "Akshan", title: "The Rogue Sentinel", accent: "#fbbf24", id: "0166", role: "Marksman", origin: "Shurima",
        lore: "มือปราบผู้ใช้ปืนตะขอวิเศษเพื่อแก้แค้นและชุบชีวิตเพื่อนร่วมทีม",
        skins: ["Akshan_0.jpg", "Akshan_1.jpg", "Akshan_2.jpg"],
        skills: [
            { k: "P", n: "Dirty Fighting", d: "ยิงสองครั้งและรับเกราะ", i: "Akshan_Passive.png" },
            { k: "Q", n: "Avengerang", d: "ปาบูมเมอแรงไกลขึ้นเรื่อยๆ", i: "Akshan_Q.png" },
            { k: "W", n: "Going Rogue", d: "พรางตัวล่าศัตรู", i: "Akshan_W.png" },
            { k: "E", n: "Heroic Swing", d: "โหนเชือกยิงปืนรัวๆ", i: "Akshan_E.png" },
            { k: "R", n: "Comeuppance", d: "ล็อคเป้าหมายชาร์จยิง", i: "Akshan_R.png" }
        ]
    },
    "Alistar": {
        name: "Alistar", title: "The Minotaur", accent: "#8b5cf6", id: "0012", role: "Tank", origin: "Noxus",
        lore: "นักรบมินอทอร์ผู้แข็งแกร่งที่สุดที่เคยมีมา",
        skins: ["Alistar_0.jpg", "Alistar_19.jpg", "Alistar_22.jpg"],
        skills: [
            { k: "P", n: "Triumphant Roar", d: "คำรามฮีลตัวเองและเพื่อน", i: "Alistar_Passive.png" },
            { k: "Q", n: "Pulverize", d: "ทุบพื้นให้ลอยขึ้น", i: "Pulverize.png" },
            { k: "W", n: "Headbutt", d: "พุ่งชนให้กระเด็น", i: "Headbutt.png" },
            { k: "E", n: "Trample", d: "เหยียบย่ำและสตั้น", i: "AlistarTrample.png" },
            { k: "R", n: "Unbreakable Will", d: "ล้างสถานะและลดดาเมจ", i: "FerociousHowl.png" }
        ]
    },
    "Ambessa": {
        name: "Ambessa", title: "Matriarch of War", accent: "#ef4444", id: "0963", role: "Fighter", origin: "Noxus",
        lore: "แม่ทัพผู้ยิ่งใหญ่แห่งตระกูล Medarda ผู้ไม่เคยรู้จักคำว่าแพ้",
        skins: ["Ambessa_0.jpg", "Ambessa_1.jpg", "Ambessa_2.jpg"],
        skills: [
            { k: "P", n: "Drakehound's Step", d: "พุ่งตัวได้หลังจากใช้สกิล", i: "Ambessa_Passive.png" },
            { k: "Q", n: "Cunning Sweep", d: "กวาดใบมีดและทุบพื้น", i: "AmbessaQ.png" },
            { k: "W", n: "Repudiation", d: "สร้างเกราะและระเบิดพลัง", i: "AmbessaW.png" },
            { k: "E", n: "Lacerate", d: "ตวัดอาวุธรอบตัวสโลว์", i: "AmbessaE.png" },
            { k: "R", n: "Public Execution", d: "พุ่งล็อคและทุ่มลงพื้น", i: "AmbessaR.png" }
        ]
    },
    "Amumu": {
        name: "Amumu", title: "The Sad Mummy", accent: "#22c55e", id: "0032", role: "Tank", origin: "Shurima",
        lore: "มัมมี่น้อยที่ออกตามหาเพื่อนไปทั่วโลก แต่ถูกสาปให้ทำลายทุกสิ่งที่รัก",
        skins: ["Amumu_0.jpg", "Amumu_23.jpg", "Amumu_34.jpg"],
        skills: [
            { k: "P", n: "Cursed Touch", d: "ดาเมจเวททำดาเมจจริงเพิ่ม", i: "Amumu_Passive.png" },
            { k: "Q", n: "Bandage Toss", d: "ปาผ้าพันแผลดึงตัว", i: "BandageToss.png" },
            { k: "W", n: "Despair", d: "ร้องไห้ทำดาเมจรอบตัว", i: "Despair.png" },
            { k: "E", n: "Tantrum", d: "ลดดาเมจและหมุนตัว", i: "Tantrum.png" },
            { k: "R", n: "Curse of the Sad Mummy", d: "ล็อคศัตรูรอบตัวเป็นวงกว้าง", i: "CurseoftheSadMummy.png" }
        ]
    },
    "Anivia": {
        name: "Anivia", title: "The Cryophoenix", accent: "#60a5fa", id: "0034", role: "Mage", origin: "Freljord",
        lore: "เทพเจ้านกน้ำแข็งผู้พิทักษ์ดินแดนหิมะ",
        skins: ["Anivia_0.jpg", "Anivia_8.jpg", "Anivia_17.jpg"],
        skills: [
            { k: "P", n: "Rebirth", d: "ตายแล้วกลายเป็นไข่คืนชีพ", i: "Anivia_P.png" },
            { k: "Q", n: "Flash Frost", d: "ยิงน้ำแข็งระเบิดสตั้น", i: "FlashFrost.png" },
            { k: "W", n: "Crystallize", d: "สร้างกำแพงน้ำแข็ง", i: "Crystallize.png" },
            { k: "E", n: "Frostbite", d: "ยิงน้ำแข็งแรง 2 เท่าใส่คนแช่แข็ง", i: "Frostbite.png" },
            { k: "R", n: "Glacial Storm", d: "พายุหิมะสโลว์ศัตรู", i: "GlacialStorm.png" }
        ]
    },
    "Annie": {
        name: "Annie", title: "The Dark Child", accent: "#f97316", id: "0001", role: "Mage", origin: "Noxus",
        lore: "เด็กหญิงผู้กุมพลังไฟมหาศาลพร้อมตุ๊กตาหมี Tibbers",
        skins: ["Annie_0.jpg", "Annie_12.jpg", "Annie_15.jpg"],
        skills: [
            { k: "P", n: "Pyromania", d: "สะสมครบ 4 สกิลจะสตั้น", i: "Annie_Passive.png" },
            { k: "Q", n: "Disintegrate", d: "บอลไฟฆ่าแล้วได้มานาคืน", i: "AnnieQ.png" },
            { k: "W", n: "Incinerate", d: "พ่นไฟเป็นรูปกรวย", i: "AnnieW.png" },
            { k: "E", n: "Molten Shield", d: "โล่ไฟสะท้อนดาเมจ", i: "AnnieE.png" },
            { k: "R", n: "Summon: Tibbers", d: "เรียกหมีไฟลงมาสู้", i: "AnnieR.png" }
        ]
    },
    "Aphelios": {
        name: "Aphelios", title: "The Weapon of the Faithful", accent: "#6366f1", id: "0523", role: "Marksman", origin: "Targon",
        lore: "นักฆ่า Lunari ผู้ใช้ปืน 5 ชนิดจากพลังดวงจันทร์",
        skins: ["Aphelios_0.jpg", "Aphelios_1.jpg", "Aphelios_9.jpg"],
        skills: [
            { k: "P", n: "The Hitman and the Seer", d: "สลับปืน 5 ชนิด", i: "ApheliosP.png" },
            { k: "Q", n: "Weapon Abilities", d: "สกิลตามชนิดปืน", i: "ApheliosQ.png" },
            { k: "W", n: "Phase", d: "สลับอาวุธหลัก/รอง", i: "ApheliosW.png" },
            { k: "E", n: "Weapon Queue", d: "แสดงคิวปืนถัดไป", i: "ApheliosE.png" },
            { k: "R", n: "Moonlight Vigil", d: "ยิงระเบิดแสงจันทร์", i: "ApheliosR.png" }
        ]
    },
    "Ashe": {
        name: "Ashe", title: "The Frost Archer", accent: "#0ea5e9", id: "0022", role: "Marksman", origin: "Freljord",
        lore: "ผู้นำผู้ใช้ธนูน้ำแข็งเพื่อรวมชาติ Freljord",
        skins: ["Ashe_0.jpg", "Ashe_11.jpg", "Ashe_23.jpg"],
        skills: [
            { k: "P", n: "Frost Shot", d: "ทุกการตีจะสโลว์", i: "Ashe_P.png" },
            { k: "Q", n: "Ranger's Focus", d: "ยิงธนูรัวกระหน่ำ", i: "AsheQ.png" },
            { k: "W", n: "Volley", d: "ยิงธนูรูปพัด", i: "Volley.png" },
            { k: "E", n: "Hawkshot", d: "ส่งเหยี่ยวเปิดแผนที่", i: "AsheSpiritOfTheHawk.png" },
            { k: "R", n: "Enchanted Crystal Arrow", d: "ศรยักษ์สตั้นข้ามแมพ", i: "EnchantedCrystalArrow.png" }
        ]
    },
    "Aurelion Sol": {
        name: "Aurelion Sol", title: "The Star Forger", accent: "#3b82f6", id: "0136", role: "Mage", origin: "Targon",
        lore: "มังกรผู้สร้างดวงดาวที่พร้อมจะทวงคืนอิสรภาพจากชาว Targon",
        skins: ["AurelionSol_0.jpg", "AurelionSol_1.jpg", "AurelionSol_11.jpg"],
        skills: [
            { k: "P", n: "Cosmic Creator", d: "สะสม Stardust เพิ่มขนาดสกิล", i: "AurelionSol_P.png" },
            { k: "Q", n: "Breath of Light", d: "พ่นไฟดวงดาวต่อเนื่อง", i: "AurelionSolQ.png" },
            { k: "W", n: "Astral Flight", d: "บินข้ามสิ่งกีดขวาง", i: "AurelionSolW.png" },
            { k: "E", n: "Singularity", d: "หลุมดำดูดและฆ่าศัตรู", i: "AurelionSolE.png" },
            { k: "R", n: "Falling Star", d: "อัญเชิญดาวตกลงมา", i: "AurelionSolR.png" }
        ]
    },
    "Aurora": {
        name: "Aurora", title: "The Witch Between Worlds", accent: "#d946ef", id: "0893", role: "Mage", origin: "Freljord",
        lore: "แม่มดผู้เดินทางข้ามมิติเพื่อรักษาสมดุลโลกวิญญาณ",
        skins: ["Aurora_0.jpg", "Aurora_1.jpg", "Aurora_2.jpg"],
        skills: [
            { k: "P", n: "Spirit Abjuration", d: "ดูดวิญญาณเพิ่มความเร็ว", i: "Aurora_Passive.png" },
            { k: "Q", n: "Two Worlds", d: "ยิงพลังข้ามมิติ", i: "AuroraQ.png" },
            { k: "W", n: "Across the Veil", d: "พรางตัวล่องหน", i: "AuroraW.png" },
            { k: "E", n: "The Weirding", d: "ระเบิดพลังวิญญาณ", i: "AuroraE.png" },
            { k: "R", n: "Between Worlds", d: "สร้างโดมกักขังศัตรู", i: "AuroraR.png" }
        ]
    },
    "Azir": {
        name: "Azir", title: "The Emperor of the Sands", accent: "#fbbf24", id: "0268", role: "Mage", origin: "Shurima",
        lore: "จักรพรรดิผู้ฟื้นจากความตายเพื่อนำความยิ่งใหญ่กลับคืนสู่ Shurima",
        skins: ["Azir_0.jpg", "Azir_4.jpg", "Azir_5.jpg"],
        skills: [
            { k: "P", n: "Shurima's Legacy", d: "สร้างป้อมปราการทราย", i: "Azir_Passive.png" },
            { k: "Q", n: "Conquering Sands", d: "สั่งทหารทรายพุ่งโจมตี", i: "AzirQ.png" },
            { k: "W", n: "Arise!", d: "เรียกทหารทรายออกมาสู้", i: "AzirW.png" },
            { k: "E", n: "Shifting Sands", d: "พุ่งตัวหาทหารทราย", i: "AzirE.png" },
            { k: "R", n: "Emperor's Divide", d: "เรียกกองทัพทหารผลักศัตรู", i: "AzirR.png" }
        ]
    }
};

function init() {
    const grid = document.getElementById('letters-grid');
    for(let i=65; i<=90; i++) {
        const char = String.fromCharCode(i);
        const div = document.createElement('div'); 
        div.className = 'letter-node'; 
        div.innerText = char;
        div.onclick = () => loadList(char); 
        grid.appendChild(div);
    }
}

function showView(id) {
    document.querySelectorAll('.view-section').forEach(v => v.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    if(id === 'index-view') {
        document.body.style.backgroundImage = "none";
        document.body.style.backgroundColor = "#000";
    }
}

function loadList(char) {
    const grid = document.getElementById('champions-grid'); 
    grid.innerHTML = "";
    const list = Object.values(champs).filter(c => c.name.toUpperCase().startsWith(char));
    if(list.length === 0) { alert(`ไม่มีข้อมูลตัวละครที่ขึ้นต้นด้วย ${char}`); return; }
    
    list.forEach(c => {
        const imgName = c.name.replace(/\s/g, ''); // สำหรับ URL รูปภาพต้องไม่มีช่องว่าง
        grid.innerHTML += `<div class="champ-card" onclick="loadProfile('${c.name}')">
            <img src="https://ddragon.leagueoflegends.com/cdn/${PATCH_VER}/img/champion/${imgName}.png">
            <div class="name-tag">${c.name.toUpperCase()}</div>
        </div>`;
    });
    showView('list-view');
}

function loadProfile(name) {
    const activeChamp = champs[name]; 
    if(!activeChamp) return;
    window.currentChamp = activeChamp;

    document.documentElement.style.setProperty('--current-accent', activeChamp.accent);
    document.getElementById('prof-name').innerText = activeChamp.name;
    document.getElementById('prof-name').style.color = activeChamp.accent;
    document.getElementById('prof-title').innerText = activeChamp.title;
    document.getElementById('prof-lore').innerText = activeChamp.lore;
    document.getElementById('prof-role').innerText = activeChamp.role;
    document.getElementById('prof-origin').innerText = activeChamp.origin;

    const sBox = document.getElementById('skin-btns'); sBox.innerHTML = "";
    activeChamp.skins.forEach((s, idx) => {
        const b = document.createElement('button'); 
        b.className = 'btn btn-skin';
        b.innerText = idx === 0 ? "Classic" : `Skin ${idx}`;
        b.onclick = () => {
            document.getElementById('mainImg').src = `https://ddragon.leagueoflegends.com/cdn/img/champion/loading/${s}`;
            document.body.style.backgroundImage = `linear-gradient(rgba(0,0,0,0.85), #000), url('https://ddragon.leagueoflegends.com/cdn/img/champion/splash/${s}')`;
            document.body.style.backgroundSize = "cover";
            document.querySelectorAll('.btn-skin').forEach(btn => btn.classList.remove('active-skin'));
            b.classList.add('active-skin');
        };
        sBox.appendChild(b);
    });

    sBox.firstChild.click(); 
    document.getElementById('profile-content').style.display = 'none';
    document.getElementById('lockBtn').disabled = false;
    document.getElementById('viewBtn').disabled = true;
    showView('profile-view');
}

function handleLock() {
    document.getElementById('lockBtn').disabled = true;
    document.getElementById('viewBtn').disabled = false;
}

function handleViewData() {
    const activeChamp = window.currentChamp;
    const fx = document.getElementById('fxOverlay');
    fx.classList.add('fx-anim');
    
    setTimeout(() => {
        const skBox = document.getElementById('skills-list'); skBox.innerHTML = "";
        activeChamp.skills.forEach(sk => {
            let vidId = activeChamp.id.toString().padStart(4, '0');
            const suffix = sk.k === 'P' ? 'P1' : `${sk.k}1`;
            const videoUrl = `https://d28xe8vt774jo5.cloudfront.net/champion-abilities/${vidId}/ability_${vidId}_${suffix}.mp4`;
            const iconType = sk.k === 'P' ? 'passive' : 'spell';

            skBox.innerHTML += `
                <div class="skill-item">
                    <div>
                        <div style="display:flex; align-items:center; gap:15px; margin-bottom:10px;">
                            <img class="skill-icon-img" src="https://ddragon.leagueoflegends.com/cdn/${PATCH_VER}/img/${iconType}/${sk.i}" 
                                 onerror="this.src='https://ddragon.leagueoflegends.com/cdn/${PATCH_VER}/img/spell/SummonerFlash.png'">
                            <div>
                                <div style="color:${activeChamp.accent}; font-weight:800;">${sk.k}</div>
                                <div style="font-weight:700;">${sk.n}</div>
                            </div>
                        </div>
                        <p style="color:#94a3b8; font-size:0.95rem;">${sk.d}</p>
                    </div>
                    <div class="skill-video-box">
                        <video loop muted autoplay playsinline src="${videoUrl}"></video>
                    </div>
                </div>`;
        });
        document.getElementById('profile-content').style.display = 'block';
        window.scrollTo({ top: 600, behavior: 'smooth' });
        fx.classList.remove('fx-anim');
    }, 800);
}

init();
</script>
</body>
</html>
