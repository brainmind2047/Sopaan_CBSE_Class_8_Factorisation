<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Sopaan · Factorisation</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,700&family=Source+Sans+3:wght@400;600;700&family=IBM+Plex+Mono:wght@500;600&display=swap">
<style>
  html,body{margin:0;} img{max-width:100%;} [hidden]{display:none !important;}
  :root{
    --navy:#16264A; --navy-2:#1F3B6B; --gold:#C79A3E; --gold-soft:#F3E7C9;
    --paper:#FBF9F4; --paper-2:#F1EAD8; --card:#FFFFFF;
    --ink:#211E1A; --ink-soft:#655D4C; --rule:#E4DCC8;
    --success:#2E8B57; --success-soft:#E1F2E7;
    --danger:#BF4B45; --danger-soft:#FAE3E1;
    --locked:#C7BEA9;
    --focus:#1F3B6B;
    --accent-text:#1F3B6B; --retry-text:#8A661F;
    color-scheme:light;
  }
  @media (prefers-color-scheme: dark){
    :root:not([data-theme="light"]){
      --navy:#0E1830; --navy-2:#16264A; --gold:#E0B75B; --gold-soft:#3A2F16;
      --paper:#141210; --paper-2:#1C1914; --card:#1C1914;
      --ink:#EDE7DA; --ink-soft:#B2A996; --rule:#3A342A;
      --success:#7BC79A; --success-soft:#1B2E22;
      --danger:#E38884; --danger-soft:#331D1B;
      --locked:#4A4436;
      --focus:#E0B75B;
      --accent-text:#E0B75B; --retry-text:#E0B75B;
      color-scheme:dark;
    }
  }
  :root[data-theme="dark"]{
    --navy:#0E1830; --navy-2:#16264A; --gold:#E0B75B; --gold-soft:#3A2F16;
    --paper:#141210; --paper-2:#1C1914; --card:#1C1914;
    --ink:#EDE7DA; --ink-soft:#B2A996; --rule:#3A342A;
    --success:#7BC79A; --success-soft:#1B2E22;
    --danger:#E38884; --danger-soft:#331D1B;
    --locked:#4A4436;
    --focus:#E0B75B;
    --accent-text:#E0B75B; --retry-text:#E0B75B;
    color-scheme:dark;
  }

  *{box-sizing:border-box;}
  body{background:var(--paper); color:var(--ink); font-family:'Source Sans 3',-apple-system,'Segoe UI',sans-serif; padding-inline:0; padding-block:0;}
  h1,h2,h3{font-family:'Fraunces',Georgia,serif; text-wrap:balance; margin:0;}
  .num{font-family:'IBM Plex Mono',ui-monospace,monospace; font-variant-numeric:tabular-nums;}

  header.brand{
    background:linear-gradient(155deg,var(--navy) 0%, var(--navy-2) 100%);
    color:#F4EFDF; padding-block:calc(20px + env(safe-area-inset-top,0px)) 18px; padding-inline:20px;
  }
  .brand-row{display:flex; align-items:center; gap:12px; max-width:900px; margin:0 auto;}
  .crest{
    width:42px; height:42px; border-radius:10px; background:var(--gold); color:var(--navy);
    display:flex; align-items:center; justify-content:center; font-family:'Fraunces',serif; font-weight:700; font-size:18px; flex-shrink:0;
  }
  .brand-name{font-size:12px; letter-spacing:.08em; text-transform:uppercase; color:var(--gold); font-weight:700;}
  .series-name{font-size:19px; font-weight:700; font-family:'Fraunces',serif;}
  .chapter-eyebrow{max-width:900px; margin:14px auto 0; font-size:12px; letter-spacing:.06em; text-transform:uppercase; color:#AEB9D6;}
  .chapter-title{font-size:28px; max-width:900px; margin:2px auto 0;}
  .chapter-sub{font-size:14.5px; color:var(--gold); font-weight:700; max-width:900px; margin:3px auto 0;}

  .chapter-progress{max-width:900px; margin:14px auto 0; display:flex; align-items:center; gap:10px;}
  .cp-track{flex:1; height:6px; border-radius:99px; background:rgba(255,255,255,.18); overflow:hidden;}
  .cp-fill{height:100%; background:var(--gold); border-radius:99px; transition:width .3s ease;}
  .cp-label{font-size:11.5px; color:#CFD7EA; white-space:nowrap;}

  .tabbar{position:sticky; top:env(safe-area-inset-top,0px); z-index:15; background:var(--paper); border-bottom:1px solid var(--rule); overflow-x:auto; white-space:nowrap;}
  .tabbar-inner{max-width:900px; margin:0 auto; display:flex; padding-inline:16px;}
  .tab-btn{font-family:inherit; font-size:14px; font-weight:600; color:var(--ink-soft); background:none; border:none; padding:13px 16px; cursor:pointer; position:relative; flex-shrink:0;}
  .tab-btn.active{color:var(--accent-text);}
  .tab-btn.active::after{content:''; position:absolute; left:12px; right:12px; bottom:0; height:3px; background:var(--gold); border-radius:3px 3px 0 0;}
  .tab-count{font-size:11px; color:var(--ink-soft); margin-left:5px;}

  .slide-progress{max-width:900px; margin:0 auto; padding:10px 16px 0; display:flex; align-items:center; gap:10px;}
  .sp-track{flex:1; height:5px; border-radius:99px; background:var(--rule); overflow:hidden;}
  .sp-fill{height:100%; background:var(--accent-text); border-radius:99px; transition:width .3s ease;}
  .sp-label{font-size:12px; color:var(--ink-soft); white-space:nowrap; font-weight:600;}

  .wrap{max-width:900px; margin:0 auto; padding:16px 16px calc(110px + env(safe-area-inset-bottom,0px));}

  .qcard{background:var(--card); border:1px solid var(--rule); border-radius:14px; padding:18px;}
  .qhead{display:flex; align-items:flex-start; gap:10px; margin-bottom:10px;}
  .qnum{width:32px; height:32px; border-radius:8px; background:var(--gold-soft); color:var(--accent-text); display:flex; align-items:center; justify-content:center; font-weight:700; font-family:'Fraunces',serif; flex-shrink:0; font-size:14px;}
  .qtext{font-size:16px; line-height:1.55; flex:1;}
  .qtag{font-size:10.5px; color:var(--gold); background:var(--gold-soft); padding:2px 7px; border-radius:99px; font-weight:700; margin-left:6px; white-space:nowrap;}
  .marks-pill{font-size:11px; font-weight:700; color:var(--ink-soft); border:1px solid var(--rule); padding:3px 9px; border-radius:99px; margin-left:auto; flex-shrink:0; white-space:nowrap;}
  .step-badge{font-size:11px; font-weight:700; color:var(--accent-text); background:var(--paper-2); border:1px solid var(--rule); padding:3px 9px; border-radius:99px; display:inline-block; margin-bottom:12px;}

  .options{display:flex; flex-direction:column; gap:8px; margin-top:10px;}
  .opt{display:flex; align-items:center; gap:10px; padding:11px 12px; border:1.5px solid var(--rule); border-radius:10px; cursor:pointer; font-size:15px; background:var(--paper);}
  .opt input{accent-color:var(--navy-2);}
  .opt.is-correct{border-color:var(--success); background:var(--success-soft);}
  .opt.is-wrong{border-color:var(--danger); background:var(--danger-soft);}
  .opt.locked{cursor:not-allowed;}

  .subpart-label{font-weight:700; font-size:12.5px; color:var(--accent-text); text-transform:uppercase; letter-spacing:.03em; margin:14px 0 6px;}
  .or-divider{text-align:center; font-size:11px; font-weight:700; letter-spacing:.08em; color:var(--ink-soft); margin:8px 0;}

  .steps{display:flex; flex-direction:column; gap:10px;}
  .step-line{font-size:14.5px; line-height:1.9; background:var(--paper); border:1px dashed var(--rule); border-radius:10px; padding:9px 12px;}
  .step-line.resolved{opacity:.9;}
  .blank-input{font-family:'IBM Plex Mono',monospace; font-size:14px; border:1.5px solid var(--rule); border-radius:6px; padding:3px 8px; width:120px; background:var(--card); color:var(--ink); margin:0 2px;}
  .blank-input:focus{outline:none; border-color:var(--focus); box-shadow:0 0 0 3px var(--gold-soft);}
  .blank-input.is-correct{border-color:var(--success); background:var(--success-soft);}
  .blank-input.is-wrong{border-color:var(--danger); background:var(--danger-soft);}
  .blank-input[disabled]{opacity:.85;}
  .reveal-note{font-size:12px; color:var(--danger); padding-left:2px; margin-top:-2px;}

  .feedback{font-size:13.5px; padding:10px 12px; border-radius:9px; margin-top:14px; display:none;}
  .feedback.show{display:block;}
  .feedback.ok{background:var(--success-soft); color:var(--success);}
  .feedback.retry{background:var(--gold-soft); color:var(--retry-text);}
  .feedback.reveal{background:var(--danger-soft); color:var(--danger);}
  .feedback.err{background:var(--danger-soft); color:var(--danger);}
  .attempts-dots{display:inline-flex; gap:4px; margin-left:2px;}
  .attempts-dots span{width:6px; height:6px; border-radius:50%; background:var(--ink-soft); opacity:.3; display:inline-block;}
  .attempts-dots span.used{opacity:1; background:var(--danger);}

  .ar-box{background:var(--paper-2); border-radius:10px; padding:10px 12px; margin-bottom:12px; font-size:13px; color:var(--ink-soft);}

  .navbar{
    position:fixed; left:0; right:0; bottom:0; z-index:30; background:var(--paper);
    border-top:1px solid var(--rule); padding:10px 16px calc(10px + env(safe-area-inset-bottom,0px));
  }
  .navbar-inner{max-width:900px; margin:0 auto; display:flex; gap:8px; flex-wrap:wrap; align-items:center;}
  .btn{font-family:inherit; font-size:13.5px; font-weight:700; padding:10px 14px; border-radius:9px; border:1.5px solid var(--rule); background:var(--card); color:var(--ink); cursor:pointer;}
  .btn:hover{border-color:var(--navy-2);}
  .btn:disabled{opacity:.4; cursor:not-allowed;}
  .btn-primary{background:var(--navy-2); color:#fff; border-color:var(--navy-2);}
  .navbar .spacer{flex:1;}

  .fab{position:fixed; right:16px; bottom:calc(74px + env(safe-area-inset-bottom,0px)); background:var(--gold); color:var(--navy); border:none; border-radius:999px; padding:11px 16px; font-family:inherit; font-weight:700; font-size:13px; display:flex; align-items:center; gap:7px; cursor:pointer; box-shadow:0 6px 16px rgba(0,0,0,.18); z-index:35;}
  .fab-badge{background:rgba(255,255,255,.55); border-radius:99px; padding:1px 7px; font-size:11px;}

  .overlay{position:fixed; inset:0; background:rgba(20,18,14,.45); z-index:50; display:none; align-items:flex-end; justify-content:center;}
  .overlay.show{display:flex;}
  .palette{background:var(--card); width:min(480px,100%); max-height:78vh; overflow-y:auto; border-radius:18px 18px 0 0; padding:18px 18px calc(18px + env(safe-area-inset-bottom,0px)); border:1px solid var(--rule); border-bottom:none;}
  @media (min-width:640px){ .overlay{align-items:center;} .palette{border-radius:18px; border-bottom:1px solid var(--rule); max-height:74vh;} }
  .palette-head{display:flex; align-items:center; justify-content:space-between; margin-bottom:6px;}
  .palette-head h2{font-size:16px;}
  .palette-close{background:none; border:none; font-size:20px; color:var(--ink-soft); cursor:pointer; padding:4px;}
  .palette-legend{display:flex; gap:12px; flex-wrap:wrap; font-size:11px; color:var(--ink-soft); margin:10px 0 14px;}
  .palette-legend span{display:inline-flex; align-items:center; gap:5px;}
  .dot{width:9px; height:9px; border-radius:50%; display:inline-block;}
  .dot.correct{background:var(--success);} .dot.revealed{background:var(--danger);}
  .dot.skipped{background:var(--gold);} .dot.locked{background:var(--locked);}
  .dot.current{background:var(--navy-2);}
  .chip-grid{display:grid; grid-template-columns:repeat(auto-fill,minmax(48px,1fr)); gap:8px;}
  .chip{border:1.5px solid var(--rule); border-radius:9px; padding:8px 4px; text-align:center; font-family:'IBM Plex Mono',monospace; font-size:12.5px; font-weight:600; cursor:pointer; background:var(--paper); color:var(--ink);}
  .chip[data-status="correct"]{border-color:var(--success); background:var(--success-soft); color:var(--success);}
  .chip[data-status="revealed"]{border-color:var(--danger); background:var(--danger-soft); color:var(--danger);}
  .chip[data-status="skipped"]{border-color:var(--gold); background:var(--gold-soft); color:var(--retry-text);}
  .chip[data-status="locked"]{border-color:var(--rule); color:var(--locked); cursor:not-allowed; background:var(--paper-2);}
  .chip[data-status="current"]{outline:2px solid var(--navy-2); outline-offset:1px;}

  .toast{position:fixed; left:50%; bottom:calc(140px + env(safe-area-inset-bottom,0px)); transform:translateX(-50%); background:var(--ink); color:var(--paper); padding:9px 16px; border-radius:99px; font-size:13px; opacity:0; pointer-events:none; transition:opacity .25s ease; z-index:60;}
  .toast.show{opacity:1;}

  .done-card{text-align:center; padding:36px 20px;}
  .done-card .big{font-size:38px; margin-bottom:6px;}
  .score-pills{display:flex; gap:10px; justify-content:center; flex-wrap:wrap; margin:16px 0;}
  .score-pill{padding:8px 16px; border-radius:99px; font-size:13px; font-weight:700;}

  footer.brandfoot{max-width:900px; margin:14px auto 0; padding:0 16px calc(110px + env(safe-area-inset-bottom,0px)); text-align:center; font-size:12px; color:var(--ink-soft);}

  .mode-pill{font-family:inherit; font-size:12.5px; font-weight:700; color:var(--navy); background:var(--gold); border:none; border-radius:99px; padding:6px 14px; cursor:pointer; margin-top:12px; display:inline-flex; align-items:center; gap:6px;}
  .mode-pick{display:flex; flex-direction:column; gap:14px; padding:8px 0 24px;}
  .mode-card{background:var(--card); border:1.5px solid var(--rule); border-radius:16px; padding:22px; cursor:pointer; text-align:left;}
  .mode-card:hover{border-color:var(--navy-2);}
  .mode-card .mode-icon{font-size:26px; margin-bottom:8px;}
  .mode-card h3{font-size:18px; margin-bottom:6px;}
  .mode-card p{font-size:13.5px; color:var(--ink-soft); line-height:1.5; margin:0;}
  .quiz-hint{font-size:12.5px; color:var(--ink-soft); background:var(--paper-2); border-radius:9px; padding:8px 12px; margin-bottom:14px;}
  .review-row{border:1px solid var(--rule); border-radius:10px; padding:11px 13px; margin-bottom:10px;}
  .review-tag{font-size:11.5px; font-weight:700; margin-bottom:4px; display:block;}
  .review-tag.ok{color:var(--success);} .review-tag.bad{color:var(--danger);} .review-tag.na{color:var(--ink-soft);}
  .review-q{font-size:13.5px; margin-bottom:6px; color:var(--ink-soft);}
  .review-ans{font-size:13px; margin-bottom:2px;}

  .who-row{max-width:900px; margin:12px auto 0; display:flex; flex-wrap:wrap; gap:8px; align-items:center;}
  .who-row .mode-pill{margin-top:0;}
  .who{display:inline-flex; flex-wrap:wrap; align-items:center; gap:6px; font-size:12.5px; color:#CFD7EA;}
  .who b{color:#F4EFDF;}
  .who button{font-family:inherit; font-size:12px; font-weight:700; color:#F4EFDF; background:rgba(255,255,255,.12); border:1px solid rgba(255,255,255,.22); border-radius:99px; padding:5px 11px; cursor:pointer;}
  .who button:hover{background:rgba(255,255,255,.2);}
  .login-card{max-width:460px; margin:8px auto 0; background:var(--card); border:1px solid var(--rule); border-radius:16px; padding:22px;}
  .login-card h2{font-size:21px; margin-bottom:4px;}
  .login-card p.lead{font-size:13.5px; color:var(--ink-soft); margin:0 0 16px; line-height:1.5;}
  .fld{display:flex; flex-direction:column; gap:5px; margin-bottom:12px;}
  .fld label{font-size:12px; font-weight:700; letter-spacing:.04em; text-transform:uppercase; color:var(--ink-soft);}
  .fld input{font-family:inherit; font-size:15px; padding:10px 12px; border:1.5px solid var(--rule); border-radius:9px; background:var(--paper); color:var(--ink);}
  .fld input:focus{outline:none; border-color:var(--focus); box-shadow:0 0 0 3px var(--gold-soft);}
  .fld-row{display:grid; grid-template-columns:1fr 1fr; gap:10px;}
  .login-err{font-size:13px; color:var(--danger); background:var(--danger-soft); border-radius:8px; padding:8px 10px; margin-bottom:12px;}
  .login-note{font-size:12px; color:var(--ink-soft); margin-top:12px; line-height:1.5;}
  .known{margin:0 0 16px; display:flex; flex-direction:column; gap:6px;}
  .known-label{font-size:12px; font-weight:700; letter-spacing:.04em; text-transform:uppercase; color:var(--ink-soft);}
  .known-btn{display:flex; justify-content:space-between; align-items:center; gap:10px; text-align:left; font-family:inherit; font-size:14.5px; padding:10px 12px; border:1.5px solid var(--rule); border-radius:10px; background:var(--paper); color:var(--ink); cursor:pointer;}
  .known-btn:hover{border-color:var(--navy-2);}
  .known-btn small{font-size:12px; color:var(--ink-soft);}
  .rec-card{background:var(--card); border:1px solid var(--rule); border-radius:14px; padding:18px; margin-bottom:14px;}
  .rec-card h2{font-size:19px; margin-bottom:10px;}
  .rec-card h3{font-size:15px; margin:4px 0 8px;}
  .rec-table-wrap{overflow-x:auto;}
  .rec-table{width:100%; border-collapse:collapse; font-size:13.5px; font-variant-numeric:tabular-nums;}
  .rec-table th{text-align:left; font-size:11.5px; text-transform:uppercase; letter-spacing:.04em; color:var(--ink-soft); padding:6px 8px; border-bottom:1px solid var(--rule); white-space:nowrap;}
  .rec-table td{padding:8px; border-bottom:1px solid var(--rule); white-space:nowrap;}
  .rec-bar{display:inline-block; width:70px; height:6px; border-radius:99px; background:var(--rule); overflow:hidden; vertical-align:middle; margin-right:6px;}
  .rec-bar i{display:block; height:100%; background:var(--success);}
  .rec-empty{font-size:13.5px; color:var(--ink-soft);}
  .sec-sub{max-width:900px; margin:0 auto; padding:10px 16px 0; font-size:12.5px; font-weight:700; color:var(--accent-text); letter-spacing:.02em;}
  .opt{flex-wrap:wrap;}
  .opt-l{font-family:'IBM Plex Mono',monospace; font-size:13px; font-weight:600; color:var(--ink-soft);}
  .opt-t{flex:1; min-width:0;}
  .opt.is-chosen:not(.is-correct):not(.is-wrong){border-color:var(--navy-2); background:var(--gold-soft);}
  .opt-tag{font-size:11px; font-weight:700; padding:2px 8px; border-radius:99px; background:var(--card); border:1px solid currentColor; white-space:nowrap;}
  .opt.is-correct .opt-tag{color:var(--success);} .opt.is-wrong .opt-tag{color:var(--danger);} .opt.is-chosen:not(.is-correct):not(.is-wrong) .opt-tag{color:var(--accent-text);}
  .chosen{font-size:13.5px; margin-top:10px; padding:8px 12px; border-radius:9px;}
  .chosen.ok{background:var(--success-soft); color:var(--success);} .chosen.bad{background:var(--danger-soft); color:var(--danger);}
  .chosen + .chosen{margin-top:6px;}
  .solution{margin-top:14px; border:1px solid var(--rule); border-left:4px solid var(--gold); background:var(--paper-2); border-radius:10px; padding:12px 14px;}
  .sol-h{font-family:'Fraunces',Georgia,serif; font-weight:700; font-size:14px; color:var(--accent-text); margin-bottom:6px;}
  .sol-line{font-size:14.5px; line-height:1.7;}
  .rev-sol summary{cursor:pointer; font-size:12.5px; font-weight:700; color:var(--accent-text); margin-top:6px;}
  .rev-sol .solution{margin-top:8px;}
  .chapter-credit{max-width:900px; margin:4px auto 0; font-size:12px; color:#CFD7EA;}
  footer.brandfoot a{color:inherit;}
  .blank-input.wide{width:260px; max-width:100%; font-family:'Source Sans 3',sans-serif;}
  .blank-input.expr{width:170px; max-width:100%;}
  /*fix-layout*/ .cp-fill,.sp-fill{width:0;} .fld{min-width:0;} .fld input{width:100%; min-width:0; box-sizing:border-box;}
  .fig{margin:6px 0 10px; display:flex; justify-content:center;}
  .figsvg{width:100%; max-width:340px; height:auto; overflow:visible;}
  .figsvg .ln,.figsvg .arm{stroke:var(--ink); stroke-width:1.6; fill:none;}
  .figsvg .arm{stroke:var(--accent-text); stroke-width:2;}
  .figsvg .pt{fill:var(--ink);}
  .figsvg .lb{fill:var(--ink); font:600 13px 'Source Sans 3',sans-serif;}
  .figsvg .al{fill:var(--accent-text); font:700 12px 'Source Sans 3',sans-serif;}
  .figsvg .wg{fill:var(--gold-soft); stroke:var(--gold); stroke-width:1.2;}
  .figsvg .wg2{fill:rgba(199,154,62,.35); stroke:var(--gold); stroke-width:1.2;}
  .figsvg .ra{fill:none; stroke:var(--ink); stroke-width:1.2;}
  .figsvg .ahd{fill:var(--ink);}
  .figsvg .pr{fill:var(--paper-2); stroke:var(--ink-soft); stroke-width:1.2;}
  .figsvg .tk{stroke:var(--ink-soft); stroke-width:.8;}
  .figsvg .po{fill:var(--ink); font:600 8.5px 'IBM Plex Mono',monospace;}
  .figsvg .pi{fill:var(--danger); font:600 7.5px 'IBM Plex Mono',monospace;}
  .figsvg .sh{fill:var(--gold-soft); stroke:var(--ink); stroke-width:1.5; stroke-linejoin:round;}
  .figsvg .sh2{fill:rgba(199,154,62,.38); stroke:var(--ink); stroke-width:1.5; stroke-linejoin:round;}
  .figsvg .sh3{fill:rgba(199,154,62,.22); stroke:var(--ink); stroke-width:1.5; stroke-linejoin:round;}
  .figsvg .none{fill:none;}
  .figsvg .hid{stroke-dasharray:5 4; fill:none;}
  .figsvg .tick{stroke:var(--ink); stroke-width:1.4; fill:none;}
  .figsvg .shd{fill:var(--accent-text); fill-opacity:.55;}
  .figsvg .cell{fill:var(--card); stroke:var(--ink); stroke-width:1.1;}
  .figsvg .dr{fill:var(--danger);} .figsvg .dw{fill:var(--card); stroke:var(--ink); stroke-width:1.2;}
  .figsvg .dotfill{fill:var(--danger);}
  .tt{border-collapse:collapse; font-size:13.5px; margin:4px auto; font-variant-numeric:tabular-nums;} .tt th,.tt td{border:1px solid var(--rule); padding:4px 9px; text-align:left;} .tt th{background:var(--paper-2); font-weight:700;} .ttw{overflow-x:auto; max-width:100%;}
  .fq{display:inline-flex; flex-direction:column; vertical-align:middle; text-align:center; font-size:.82em; line-height:1.12; margin:0 2px;}
  .fq>span:first-child{border-bottom:1.5px solid currentColor; padding:0 2px;}
  .fq>span:last-child{padding:0 2px;}
  .mx{white-space:nowrap;}
  .blank-input.fr{width:110px;}
  @media (prefers-reduced-motion:reduce){ *{transition:none !important;} }
.datline{display:block;margin-top:8px;padding:8px 10px;border-radius:8px;background:var(--paper-2);font:500 14px/1.7 'IBM Plex Mono',monospace;word-spacing:2px;overflow-wrap:anywhere;}

  .theory{display:flex;flex-direction:column;gap:16px;padding-bottom:40px;}
  .hub{display:grid;grid-template-columns:1fr;gap:12px;}
  @media (min-width:640px){ .hub{grid-template-columns:repeat(3,1fr);} }
  .hub-card{background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:16px;display:flex;flex-direction:column;gap:8px;}
  .hub-card h3{font-family:'Fraunces',serif;font-size:18px;margin:0;color:var(--accent-text);}
  .hub-card p{margin:0;font-size:14px;color:var(--ink-soft);line-height:1.5;}
  .hub-btns{display:flex;flex-wrap:wrap;gap:8px;margin-top:auto;}
  .hub-btn{min-height:40px;border:1.5px solid var(--rule);background:var(--paper-2);color:var(--ink);border-radius:10px;padding:8px 12px;font:600 14px 'Source Sans 3',sans-serif;cursor:pointer;text-align:left;}
  .hub-btn:hover{border-color:var(--gold);}
  .hub-btn.primary{background:var(--navy);color:#F4EFDF;border-color:var(--navy);}
  .note{background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:18px;scroll-margin-top:120px;}
  .note h2{font-family:'Fraunces',serif;font-size:22px;margin:0 0 4px;color:var(--ink);}
  .note .lt{font-size:13.5px;color:var(--ink-soft);margin:0 0 12px;}
  .note .lt b{color:var(--accent-text);}
  .note h4{font:700 15px 'Source Sans 3',sans-serif;margin:16px 0 6px;color:var(--accent-text);}
  .note p,.note li{font-size:15.5px;line-height:1.6;}
  .note ul{padding-left:20px;margin:6px 0;}
  .keybox{background:var(--gold-soft);border-left:4px solid var(--gold);border-radius:8px;padding:10px 12px;margin:10px 0;font-size:15px;line-height:1.6;}
  .keybox b{color:var(--ink);}
  .ex{background:var(--paper-2);border-radius:10px;padding:12px;margin:10px 0;}
  .ex .exh{font-family:'Fraunces',serif;font-weight:700;color:var(--accent-text);margin-bottom:4px;}
  .ex .exl{font-size:15px;line-height:1.7;}
  .ttab{width:100%;border-collapse:collapse;font-size:14.5px;margin:8px 0;}
  .ttab th,.ttab td{border:1px solid var(--rule);padding:7px 8px;text-align:left;vertical-align:top;}
  .ttab th{background:var(--paper-2);}
  .tscroll{overflow-x:auto;}
  .mono{font-family:'IBM Plex Mono',monospace;font-size:.92em;}
  @media (max-width:480px){ .ttab{font-size:13.5px;} .ttab th,.ttab td{padding:6px;} }
  .note .figsvg{max-width:100%;height:auto;display:block;margin:8px auto;}


  .rep-top{display:flex;flex-wrap:wrap;gap:18px;align-items:center;justify-content:center;margin-top:8px;}
  .rep-legend{flex:1;min-width:220px;display:flex;flex-direction:column;gap:8px;}
  .rep-cap{font:700 13px 'Source Sans 3',sans-serif;color:var(--ink-soft);text-transform:uppercase;letter-spacing:.05em;}
  .rep-li{display:flex;align-items:center;gap:6px;font-size:15px;}
  .rep-n{margin-left:auto;white-space:nowrap;padding-left:8px;font-family:'IBM Plex Mono',monospace;font-weight:600;}
  .sw{display:inline-block;width:12px;height:12px;border-radius:3px;flex:none;}
  .rep-grid{display:grid;grid-template-columns:1fr;gap:12px;}
  @media (min-width:640px){ .rep-grid{grid-template-columns:1fr 1fr;} }
  .rep-card{background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:14px;display:flex;flex-direction:column;gap:10px;}
  .rep-h{display:flex;align-items:center;gap:8px;font:700 16px 'Source Sans 3',sans-serif;}
  .crit{display:inline-grid;place-items:center;width:28px;height:28px;border-radius:50%;color:var(--card);font:700 15px Fraunces,serif;flex:none;}
  .rep-row{display:flex;gap:14px;align-items:center;}
  .rep-stats{display:flex;flex-direction:column;gap:5px;font-size:14.5px;}
  .rep-stats div{display:flex;align-items:center;gap:6px;}
  .rep-lvl{margin-top:4px;color:var(--accent-text);} .rep-lvl span{color:var(--ink-soft);font-size:13px;}
  .rep-note{font-size:13px;color:var(--ink-soft);}


  .skip-chips{display:flex;flex-wrap:wrap;gap:8px;justify-content:center;margin:12px 0;}
  .skip-chips .chip{min-width:48px;min-height:40px;cursor:pointer;border:1.5px solid var(--gold);background:var(--gold-soft);color:var(--retry-text);border-radius:10px;font:600 14px 'IBM Plex Mono',monospace;}
  .skip-btns{display:flex;flex-wrap:wrap;gap:10px;justify-content:center;margin-top:12px;}


  .kb-toggle{border:none;background:transparent;font-size:15px;cursor:pointer;padding:0 2px;vertical-align:middle;opacity:.7;}
  .vkb{position:fixed;left:0;right:0;bottom:0;z-index:80;background:var(--paper-2);border-top:1px solid var(--rule);box-shadow:0 -6px 20px rgba(0,0,0,.12);padding:6px 6px 8px;display:none;}
  .vkb.show{display:block;}
  .vkb-top{display:flex;justify-content:space-between;align-items:center;max-width:640px;margin:0 auto 4px;font:600 12px 'Source Sans 3',sans-serif;color:var(--ink-soft);}
  .vkb-link{border:none;background:none;color:var(--accent-text);font:600 12px 'Source Sans 3',sans-serif;cursor:pointer;text-decoration:underline;}
  .vkb-row{display:flex;gap:5px;max-width:640px;margin:0 auto 5px;}
  .vkb-k{flex:1;min-height:42px;border:1px solid var(--rule);border-radius:8px;background:var(--card);color:var(--ink);font:600 17px 'IBM Plex Mono',monospace;cursor:pointer;padding:0;}
  .vkb-k.fn{font:600 13px 'Source Sans 3',sans-serif;background:var(--gold-soft);}
  .vkb-k.done{background:var(--navy);color:#F4EFDF;border-color:var(--navy);}
  .vkb-k:active{transform:scale(.96);}
  body.kb-open .wrap{padding-bottom:360px;}
  body.kb-open .fab{display:none;}
  .tool-bar{display:flex;gap:8px;flex-wrap:wrap;margin:-4px 0 10px;}
  .tool-btn{min-height:36px;border:1.5px solid var(--gold);background:var(--gold-soft);color:var(--ink);border-radius:18px;padding:4px 12px;font:600 13.5px 'Source Sans 3',sans-serif;cursor:pointer;}
  .tool-panel{position:fixed;z-index:85;background:var(--card);border:1px solid var(--rule);border-radius:14px;box-shadow:0 10px 30px rgba(0,0,0,.25);display:none;}
  .tool-panel.show{display:block;}
  .tp-head{display:flex;align-items:center;gap:8px;padding:8px 10px;border-bottom:1px solid var(--rule);font:600 14px 'Source Sans 3',sans-serif;}
  .tp-note{font-size:12px;color:var(--ink-soft);font-weight:500;}
  .tp-x{margin-left:auto;border:1px solid var(--rule);background:var(--paper-2);color:var(--ink);border-radius:8px;min-height:32px;padding:2px 10px;cursor:pointer;font:600 13px 'Source Sans 3',sans-serif;}
  .tp-x + .tp-x{margin-left:0;}
  .calc{right:12px;bottom:84px;width:min(330px,calc(100vw - 24px));}
  body.calc-open .wrap{padding-bottom:440px;}
  body.calc-open .fab{display:none;}
  @media (max-width:640px){ .calc{left:6px;right:6px;width:auto;} .calc-keys button{min-height:36px;} }
  .calc-disp{padding:8px 12px;text-align:right;background:var(--paper-2);}
  .calc-expr{font:500 13px 'IBM Plex Mono',monospace;color:var(--ink-soft);min-height:18px;word-break:break-all;}
  .calc-res{font:600 24px 'IBM Plex Mono',monospace;color:var(--ink);word-break:break-all;}
  .calc-keys{display:grid;grid-template-columns:repeat(6,1fr);gap:5px;padding:8px;}
  .calc-keys button{min-height:40px;border:1px solid var(--rule);border-radius:8px;background:var(--card);color:var(--ink);font:600 14px 'IBM Plex Mono',monospace;cursor:pointer;padding:0;}
  .calc-keys button.fn{background:var(--gold-soft);font-family:'Source Sans 3',sans-serif;font-size:13px;}
  .calc-keys button.eq{grid-column:span 2;background:var(--navy);color:#F4EFDF;border-color:var(--navy);}
  .desmos{left:50%;top:50%;transform:translate(-50%,-50%);width:min(760px,calc(100vw - 16px));height:min(560px,calc(100vh - 90px));flex-direction:column;}
  .desmos.show{display:flex;}
  #desmosBox{flex:1;min-height:0;border-radius:0 0 14px 14px;overflow:hidden;}
  .desmos-msg{padding:24px;text-align:center;color:var(--ink-soft);}
  .chip[data-status="skipped"]{cursor:pointer;}
  .chip[data-status="answered"]{border-color:var(--accent-text);color:var(--accent-text);}


  button.crest-home{border:none;padding:0;position:relative;cursor:pointer;transition:transform .15s ease;}
  button.crest-home:hover,button.crest-home:focus-visible{transform:scale(1.06);outline:2px solid var(--gold);outline-offset:2px;}
  .crest-h{position:absolute;right:-7px;bottom:-7px;width:18px;height:18px;border-radius:50%;background:#F4EFDF;color:var(--navy);font:700 12px/18px 'Source Sans 3',sans-serif;text-align:center;box-shadow:0 1px 3px rgba(0,0,0,.3);}
  .brand .brand-row{position:relative;}
  .bmasw{position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);display:flex;flex-shrink:0;white-space:nowrap;align-items:center;gap:6px;background:rgba(255,255,255,.12);border:1px solid rgba(255,255,255,.25);border-radius:99px;padding:5px 14px;color:#F4EFDF;}
  .bmasw-ic{font-size:16px;} .bmasw-t{font:600 20px 'IBM Plex Mono',monospace;letter-spacing:.02em;min-width:62px;text-align:center;}
  .bmasw-b{border:none;background:rgba(255,255,255,.18);color:#F4EFDF;border-radius:50%;width:30px;height:30px;cursor:pointer;font-size:13px;}
  .bmasw.off{display:none;}
  @media (max-width:560px){ .bmasw{position:static;transform:none;margin-left:auto;padding:4px 5px 4px 8px;gap:4px;} .bmasw-ic{display:none;} .bmasw-t{font-size:16px;min-width:48px;} .bmasw-b{width:26px;height:26px;} .brand .series-name{font-size:15px;} .brand .brand-name{font-size:10.5px;} .brand .brand-row>div:not(.bmasw){min-width:0;} }
  .bmasw-done{font:600 15px 'IBM Plex Mono',monospace;color:var(--accent-text);margin:4px 0 8px;}
  .sp-fab{position:fixed;left:14px;bottom:84px;z-index:60;border:1.5px solid var(--navy-2);background:var(--card);color:var(--ink);border-radius:99px;padding:9px 14px;font:700 14px 'Source Sans 3',sans-serif;box-shadow:0 4px 14px rgba(0,0,0,.15);cursor:pointer;}
  .sp-fab.on{background:var(--navy);color:#F4EFDF;}
  body.kb-open .sp-fab{display:none;}
  .sp-canvas{position:absolute;z-index:55;display:none;touch-action:none;cursor:crosshair;}
  .sp-canvas.passive{pointer-events:none;cursor:default;}
  .sp-bar{position:fixed;top:8px;left:8px;right:8px;margin:0 auto;width:max-content;z-index:90;display:none;align-items:center;gap:6px;background:var(--card);border:1px solid var(--rule);border-radius:14px;padding:6px 8px;box-shadow:0 6px 20px rgba(0,0,0,.2);max-width:calc(100vw - 16px);flex-wrap:wrap;justify-content:center;}
  .sp-lbl{font:700 13px 'Source Sans 3',sans-serif;color:var(--ink-soft);margin-right:2px;}
  .sp-pen,.sp-tool{width:38px;height:38px;border-radius:10px;border:1.5px solid var(--rule);background:var(--paper);cursor:pointer;display:inline-flex;align-items:center;justify-content:center;font-size:17px;padding:0;color:var(--ink);}
  .sp-pen i{width:20px;height:20px;border-radius:50%;display:block;}
  .sp-pen.on,.sp-tool.on{border-color:var(--navy-2);box-shadow:0 0 0 3px var(--gold-soft);}
  @media (max-width:560px){ .sp-lbl{display:none;} .sp-pen,.sp-tool{width:34px;height:34px;} .sp-fab span{display:none;} }

  /* larger reading sizes */
  .chapter-eyebrow{font-size:13.5px;}
  .chapter-title{font-size:36px;line-height:1.15;}
  .chapter-sub{font-size:16.5px;}
  .tab-btn{font-size:15.5px;}
  .sec-sub{font-size:15px;}
  .qtext{font-size:19px;line-height:1.55;}
  .qnum{width:36px;height:36px;font-size:16px;}
  .opt{font-size:17.5px;padding:12px 14px;}
  .step-line{font-size:17.5px;line-height:2;}
  .blank-input{font-size:16px;}
  .sol-line{font-size:16.5px;}
  .feedback{font-size:15px;}
  .note h2{font-size:28px;}
  .note h4{font-size:19px;}
  .note p,.note li{font-size:17.5px;line-height:1.65;}
  .note .lt{font-size:15.5px;}
  .ex .exh{font-size:18px;} .ex .exl{font-size:16.5px;}
  .keybox{font-size:16.5px;}
  .ttab{font-size:15.5px;}
  .hub-card h3{font-size:21px;} .hub-card p{font-size:15.5px;} .hub-btn{font-size:15px;}
  @media (max-width:480px){ .chapter-title{font-size:30px;} .qtext{font-size:18px;} .opt,.step-line{font-size:16.5px;} .note h2{font-size:24px;} .note p,.note li{font-size:16.5px;} }
</style>
</head>
<body>
<header class="brand">
  <div class="brand-row">
    <div class="crest">BM</div>
    <div>
      <div class="brand-name">Brain &amp; Mind Academy</div>
      <div class="series-name">Sopaan <span style="font-weight:400;font-size:14px;color:#CFD7EA;">Practice Series</span></div>
    </div>
  </div>
  <div class="chapter-eyebrow">Class 8 Mathematics · Chapter 8</div>
  <div class="chapter-title">Factorisation</div>
  <div class="chapter-sub">Theory Notes · Practice by Learning Objective · Learning Assessment</div><div class="chapter-credit">Mixed multiple-choice and fill-in-the-blank practice · Chapter 8</div>
  <div class="chapter-progress">
    <div class="cp-track"><div class="cp-fill" id="cpFill"></div></div>
    <div class="cp-label" id="cpLabel">0% complete</div>
  </div>
  <div class="who-row"><button class="mode-pill" id="modePill" hidden>Choose a mode</button><span class="who" id="whoBar" hidden></span></div>
</header>

<nav class="tabbar"><div class="tabbar-inner" id="tabbar"></div></nav>
<div class="sec-sub" id="secSub"></div><div class="slide-progress"><div class="sp-track"><div class="sp-fill" id="spFill"></div></div><div class="sp-label" id="spLabel">Question 1 of 49</div></div>
<div class="wrap" id="wrap"></div>
<footer class="brandfoot">Brain &amp; Mind Academy · Sopaan Practice Series · Class 8 Mathematics · Chapter 8<br>Chapter follows the Class 8 mathematics syllabus (New Enjoying Mathematics, Class 8). Theory notes, questions, learning assessments and worked solutions are written by Brain &amp; Mind Academy.</footer>

<div class="navbar"><div class="navbar-inner" id="navbarInner"></div></div>
<button class="fab" id="paletteFab"><span>Questions</span><span class="fab-badge" id="fabBadge">0/49</span></button>

<div class="overlay" id="overlay">
  <div class="palette" role="dialog" aria-label="Question palette">
    <div class="palette-head"><h2 id="paletteTitle">Question palette</h2><button class="palette-close" id="paletteClose">&times;</button></div>
    <div class="palette-legend">
      <span><i class="dot current"></i>Current</span><span><i class="dot correct"></i>Correct</span>
      <span><i class="dot revealed"></i>Revealed</span><span><i class="dot skipped"></i>Skipped</span>
      <span><i class="dot locked"></i>Locked</span>
    </div>
    <div class="chip-grid" id="paletteBody"></div>
  </div>
</div>
<div class="toast" id="toast"></div>

<script>
(function(){
"use strict";

/* ================= audio ================= */
var audioCtx=null;
function ensureAudio(){ if(!audioCtx){ try{ audioCtx=new (window.AudioContext||window.webkitAudioContext)(); }catch(e){} } return audioCtx; }
function playTone(freqs,dur,type){
  var ctx=ensureAudio(); if(!ctx) return;
  try{
    var t0=ctx.currentTime;
    freqs.forEach(function(f,i){
      var o=ctx.createOscillator(), g=ctx.createGain();
      o.type=type||'sine'; o.frequency.value=f;
      var start=t0+i*dur;
      g.gain.setValueAtTime(0.0001,start);
      g.gain.exponentialRampToValueAtTime(0.16,start+0.02);
      g.gain.exponentialRampToValueAtTime(0.0001,start+dur);
      o.connect(g); g.connect(ctx.destination);
      o.start(start); o.stop(start+dur+0.02);
    });
  }catch(e){}
}
function playSuccess(){ playTone([523.25,659.25,783.99],0.11,'sine'); }
function playWrong(){ playTone([220,185],0.14,'square'); }
function playReveal(){ playTone([300,220],0.16,'triangle'); }

/* ================= helpers ================= */
function norm(s){
  return String(s===undefined||s===null?'':s).trim().toLowerCase().replace(/\s+/g,'')
    .replace(/[×∗*·]/g,'x').replace(/÷/g,'/').replace(/–|—|−/g,'-')
    .replace(/[²]/g,'^2').replace(/[³]/g,'^3').replace(/[⁴]/g,'^4').replace(/[⁵]/g,'^5')
    .replace(/[⁶]/g,'^6').replace(/[⁷]/g,'^7').replace(/[⁸]/g,'^8').replace(/[⁹]/g,'^9')
    .replace(/\^/g,'');
}
function parseNum(s){
  if(s===undefined||s===null) return null;
  var t=String(s).trim().replace(/,/g,'').replace(/[−–—]/g,'-').replace(/\s+/g,''); if(t==='') return null;
  var neg=false; if(t[0]==='-'){neg=true;t=t.slice(1);}
  var m=t.match(/^(\d+)\/(\d+)$/);
  if(m){ var v=parseInt(m[1],10)/parseInt(m[2],10); return neg?-v:v; }
  if(/^\d+(\.\d+)?$/.test(t)){ var v2=parseFloat(t); return neg?-v2:v2; }
  return null;
}
/* ---- expression checker: compares answers like (P-2w)/2 and P/2-w at random values ---- */
function exprPrep(s){
  return String(s).toLowerCase().replace(/\s+/g,'').replace(/^[a-z]\(x\)=/i,'').replace(/^y=/i,'').replace(/[×∗·]/g,'*').replace(/÷/g,'/').replace(/[−–—]/g,'-')
    .replace(/²/g,'^2').replace(/³/g,'^3').replace(/⁴/g,'^4').replace(/⁵/g,'^5').replace(/⁶/g,'^6').replace(/⁷/g,'^7').replace(/⁸/g,'^8').replace(/⁹/g,'^9').replace(/π/g,'#').replace(/pi/gi,'#').replace(/½/g,'(1/2)');
}
function exprCompile(src){
  var s=exprPrep(src), i=0, toks=[], absDepth=0;
  while(i<s.length){
    var ch=s[i];
    if(/[0-9.]/.test(ch)){ var j=i; while(j<s.length && /[0-9.]/.test(s[j])) j++; toks.push({k:'n',v:parseFloat(s.slice(i,j))}); i=j; continue; }
    if(/[a-zA-Z#]/.test(ch)){ toks.push({k:'v',v:ch}); i++; continue; }
    if(ch==='|'){ var pv=toks[toks.length-1]; var opn=(absDepth===0)||!pv||('+-*/^(['.indexOf(pv.k)>=0); if(opn){ absDepth++; toks.push({k:'['}); } else { absDepth--; toks.push({k:']'}); } i++; continue; }
    if('+-*/^()'.indexOf(ch)>=0){ toks.push({k:ch}); i++; continue; }
    return null;
  }
  var out=[];
  for(var t=0;t<toks.length;t++){
    var a=toks[t], b=out[out.length-1];
    if(b && (b.k==='n'||b.k==='v'||b.k===')'||b.k===']') && (a.k==='n'||a.k==='v'||a.k==='('||a.k==='[')) out.push({k:'&'});
    out.push(a);
  }
  var p=0;
  function peek(){ return out[p]; }
  function parseE(){ var n=parseT(); while(peek() && (peek().k==='+'||peek().k==='-')){ var o=out[p++].k, r=parseT(); n=(function(l,r,o){return function(e){ return o==='+'?l(e)+r(e):l(e)-r(e); };})(n,r,o); } return n; }
  function parseI(){ var n=parseU(); while(peek() && peek().k==='&'){ p++; var r=parseP(); n=(function(l,r){return function(e){ return l(e)*r(e); };})(n,r); } return n; }
  function parseT(){ var n=parseI(); while(peek() && (peek().k==='*'||peek().k==='/')){ var o=out[p++].k, r=parseI(); n=(function(l,r,o){return function(e){ return o==='*'?l(e)*r(e):l(e)/r(e); };})(n,r,o); } return n; }
  function parseU(){ if(peek() && peek().k==='-'){ p++; var u=parseU(); return function(e){ return -u(e); }; } if(peek() && peek().k==='+'){ p++; return parseU(); } return parseP(); }
  function parseP(){ var b=parseA(); if(peek() && peek().k==='^'){ p++; var x=parseU(); return function(e){ return Math.pow(b(e),x(e)); }; } return b; }
  function parseA(){
    var t=out[p++]; if(!t) throw 0;
    if(t.k==='n') return function(){ return t.v; };
    if(t.k==='v') return t.v==='#' ? function(){ return Math.PI; } : function(e){ return e[t.v]; };
    if(t.k==='('){ var n=parseE(); if(!peek()||peek().k!==')') throw 0; p++; return n; }
    if(t.k==='['){ var m=parseE(); if(!peek()||peek().k!==']') throw 0; p++; return function(e){ return Math.abs(m(e)); }; }
    throw 0;
  }
  try{ var f=parseE(); if(p!==out.length) return null; return f; }catch(e){ return null; }
}
function exprEqual(input,answer){
  var f=exprCompile(input), g=exprCompile(answer); if(!f||!g) return false;
  var hits=0;
  for(var trial=0;trial<8;trial++){
    var env={}; 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('').forEach(function(c){ env[c]=1.3+Math.random()*4.1; });
    var x=f(env), y=g(env);
    if(!isFinite(x)||!isFinite(y)) continue;
    if(Math.abs(x-y)>1e-7*Math.max(1,Math.abs(x),Math.abs(y))) return false;
    hits++;
  }
  return hits>=3;
}
function wordsNorm(s){ return String(s||'').toLowerCase().replace(/\band\b/g,' ').replace(/[^a-z]/g,''); }
function listNorm(s){ return (String(s||'').replace(/[−–—]/g,'-').replace(/(\d) (?=\d{3}\b)/g,'$1').match(/-?\d+(\.\d+)?/g)||[]).join(','); }
function powNorm(s){ return String(s||'').toLowerCase().replace(/\s+/g,'').replace(/[×∗*·.]/g,'x').replace(/[⁰¹²³⁴⁵⁶⁷⁸⁹]+/g,function(m){ return '^'+m.split('').map(function(c){ return '⁰¹²³⁴⁵⁶⁷⁸⁹'.indexOf(c); }).join(''); }).replace(/\^1(?!\d)/g,''); }
function fr(s){ return String(s).replace(/\{(\d+) (\d+)\/(\d+)\}/g,'<span class="mx">$1<span class="fq"><span>$2</span><span>$3</span></span></span>').replace(/\{(\d+)\/(\d+)\}/g,'<span class="fq"><span>$1</span><span>$2</span></span>'); }
function gcdI(a,b){ a=Math.abs(a); b=Math.abs(b); while(b){ var t=a%b; a=b; b=t; } return a; }
function fracParse(s){ s=String(s===undefined||s===null?'':s).trim().replace(/[\u2044\u2215\u00f7]/g,'/').replace(/\s+/g,' ').replace(/\s*\/\s*/g,'/'); var m;
  if((m=s.match(/^(-?\d+)$/))) return {v:+m[1],form:'int',n:+m[1],d:1};
  if((m=s.match(/^(-?\d+)\/(\d+)$/))){ if(+m[2]===0) return null; return {v:m[1]/m[2],form:'frac',n:+m[1],d:+m[2]}; }
  if((m=s.match(/^(-?)(\d+)(?: |\+)(\d+)\/(\d+)$/))){ if(+m[4]===0) return null; var sg=m[1]?-1:1; return {v:sg*(+m[2]+m[3]/m[4]),form:'mixed',w:sg*m[2],n:+m[3],d:+m[4]}; }
  if((m=s.match(/^-?\d*\.\d+$/))) return {v:parseFloat(s),form:'dec'};
  return null; }
function fracOK(mode,input,answer){
  if(mode==='flist'){ var A=String(input).split(/[,;]/).map(function(x){return x.trim();}).filter(Boolean), K=String(answer).split(/[,;]/).map(function(x){return x.trim();});
    if(A.length!==K.length) return false; return A.every(function(x,i){ var a=fracParse(x), k=fracParse(K[i]); return a&&k&&Math.abs(a.v-k.v)<1e-9; }); }
  var a=fracParse(input), k=fracParse(answer); if(!a||!k) return false;
  var same=Math.abs(a.v-k.v)<1e-9; if(!same) return false;
  if(mode==='fv') return true;
  if(mode==='dec') return a.form==='dec'||a.form==='int';
  if(mode==='fe') return (a.form==='frac'&&k.form==='frac'&&a.n===k.n&&a.d===k.d)||(a.form==='int'&&k.form==='int');
  if(mode==='fi') return a.form==='frac'||(a.form==='int'&&k.form==='int');
  if(mode==='fl') return a.form==='int'||(a.form==='frac'&&gcdI(a.n,a.d)===1)||(a.form==='mixed'&&a.n<a.d&&gcdI(a.n,a.d)===1);
  if(mode==='fm') return a.form==='int'||(a.form==='mixed'&&a.n>0&&a.n<a.d&&gcdI(a.n,a.d)===1);
  return false; }
function answerMatches(input,answer,accept,expr){
  if(expr==='dec'||expr==='fv'||expr==='fe'||expr==='fl'||expr==='fm'||expr==='fi'||expr==='flist'){ if(input===undefined||input===null||String(input).trim()==='') return false; return fracOK(expr,input,answer); }
  if(input===undefined||input===null||String(input).trim()==='') return false;
  if(expr==='words'){ return [answer].concat(accept||[]).some(function(a){ if(/\d/.test(a)) return norm(a)===norm(input); var w=wordsNorm(a); return w!=='' && w===wordsNorm(input); }); }
  if(expr==='list'){ return [answer].concat(accept||[]).some(function(a){ return listNorm(a)===listNorm(input); }); }
  if(expr==='pow'){ return [answer].concat(accept||[]).some(function(a){ return powNorm(a)===powNorm(input); }); }
  if(expr==='time'){ var tn=function(x){ var t=String(x).toLowerCase().replace(/[\s.]/g,'').replace(/[:h]/g,''); if(/^\d{3}(am|pm)?$/.test(t)) t='0'+t; return t; }; return [answer].concat(accept||[]).some(function(a){ return tn(a)===tn(input); }); }
  if(expr==='glist'){ var gn=function(x){ return String(x).toUpperCase().split(/[\s,;]+/).filter(Boolean).sort().join(','); }; return [answer].concat(accept||[]).some(function(a){ return gn(a)===gn(input); }); }
  if(expr==='coord'){ var cn=function(x){ return String(x).replace(/[\u2212\u2013\u2014]/g,'-').replace(/[\s()\[\]]/g,''); }; return [answer].concat(accept||[]).some(function(a){ return cn(a)===cn(input); }); }
  if(expr==='dlist'){ var dn=function(x){ return (String(x).replace(/[−–—]/g,'-').match(/-?\d*\.?\d+/g)||[]).map(Number).join(','); }; return [answer].concat(accept||[]).some(function(a){ return dn(a)===dn(input); }); }
  if(expr==='set'){ var sn=function(x){ return (String(x).match(/-?\d+/g)||[]).map(Number).sort(function(a,b){return a-b;}).join(','); }; return [answer].concat(accept||[]).some(function(a){ return sn(a)===sn(input); }); }
  if(expr==='primes'){ var pn=function(x){ var t=powNorm(x).replace(/[^0-9x^]/g,''); var out=[]; t.split('x').forEach(function(tok){ if(!tok) return; var m=tok.split('^'); var b=+m[0], e=m[1]?+m[1]:1; for(var i=0;i<e;i++) out.push(b); }); return out.sort(function(a,b){return a-b;}).join(','); }; return [answer].concat(accept||[]).some(function(a){ return pn(a)===pn(input); }); }
  if(expr==='angle'){ var an=function(x){ return String(x).toUpperCase().replace(/[^A-Z]/g,''); }; var k=an(answer), v=an(input); return v===k || v===k.split('').reverse().join(''); }
  if(expr==='trans'){ var tv=function(x){ var s=String(x).toLowerCase().replace(/[−–—]/g,'-').replace(/\b(units?|squares?|and|then|steps?|to|the|a)\b/g,' ').replace(/[,;.]/g,' '); var re=/(\d+)\s*(right|left|up|down|r|l|u|d)\b/g, m, dx=0, dy=0, n=0; while((m=re.exec(s))){ var v=+m[1], d=m[2][0]; if(d==='r')dx+=v; else if(d==='l')dx-=v; else if(d==='u')dy+=v; else dy-=v; n++; } if(!n||s.replace(re,'').replace(/\s+/g,'')!=='') return null; return dx+','+dy; }; var ti=tv(input); return ti!==null && [answer].concat(accept||[]).some(function(a){ return tv(a)===ti; }); }
  if(expr==='rot'){ var rv=function(x){ var s=String(x).toLowerCase().replace(/quarter[\s-]*turn/g,'90').replace(/half[\s-]*turn/g,'180').replace(/three[\s-]*quarter[\s-]*turn/g,'270'); var m=s.match(/\b(90|180|270)\b/); if(!m) return null; var a=+m[1]; if(a===180) return '180'; var dir=/anti|counter|acw|ccw/.test(s)?-1:(/clockwise|\bcw\b/.test(s)?1:0); if(!dir) return null; return String(((a*dir)%360+360)%360); }; var ri=rv(input); return ri!==null && ri===rv(answer); }
  if(expr==='expanded'){ var f=function(x){ return String(x).replace(/\s+/g,'').replace(/,/g,''); }; return [answer].concat(accept||[]).some(function(a){ return f(a)===f(input); }); }
  if(expr===true && input!==undefined && input!==null && String(input).trim()!==''){
    if(exprEqual(input,answer)) return true;
    var alt=String(input).replace(/\/\s*(\d+(?:\.\d+)?)\s*([a-zA-Z(])/g,'/$1*$2'); if(alt!==String(input) && exprEqual(alt,answer)) return true;
    return (accept||[]).some(function(a){ return exprEqual(input,a); });
  }
  if(input===undefined||input===null||String(input).trim()==='') return false;
  var n1=parseNum(input), n2=parseNum(answer);
  if(n1!==null && n2!==null){ if(Math.abs(n1-n2)<1e-3) return true; return (accept||[]).some(function(a){ var n3=parseNum(a); return n3!==null && Math.abs(n1-n3)<1e-3; }); }
  var cands=[answer].concat(accept||[]); var ni=norm(input);
  for(var i=0;i<cands.length;i++){ if(norm(cands[i])===ni) return true; }
  return false;
}
function esc(s){ return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;'); }

/* ================= DATA ================= */
var AR_OPTIONS = ["Both A and R are true, and R is the correct explanation of A","Both A and R are true, but R is NOT the correct explanation of A","A is true but R is false","A is false but R is true"];
var THEORY = "<div class=\"hub\"><div class=\"hub-card\"><h3>📖 Theory notes</h3><p>Explained notes for every part of the chapter, following the book: factors and HCF, taking out the HCF, regrouping, identities and splitting the middle term, with solved examples.</p><div class=\"hub-btns\"><button class=\"hub-btn\" data-jump=\"n81\">8.1 notes</button><button class=\"hub-btn\" data-jump=\"n82\">8.2 notes</button><button class=\"hub-btn\" data-jump=\"n83\">8.3 notes</button><button class=\"hub-btn\" data-jump=\"n84\">8.4 notes</button><button class=\"hub-btn\" data-jump=\"n85\">8.5 notes</button><button class=\"hub-btn\" data-jump=\"n86\">8.6 notes</button></div></div><div class=\"hub-card\"><h3>🎯 Practice by learning objective</h3><p>Every Looking Back, Example, Try This and Exercise question of the chapter, one sheet per objective, mixing multiple-choice and fill-in-the-blank questions. The bold tag shows where each question is in the book.</p><div class=\"hub-btns\"><button class=\"hub-btn\" data-go=\"s1\">8.1 · Factors and HCF of monomials</button><button class=\"hub-btn\" data-go=\"s2\">8.2 · Factorisation using the HCF</button><button class=\"hub-btn\" data-go=\"s3\">8.3 · Factorisation by regrouping</button><button class=\"hub-btn\" data-go=\"s4\">8.4 · Factorisation using identities</button><button class=\"hub-btn\" data-go=\"s5\">8.5 · Trinomials x² + ax + b</button><button class=\"hub-btn\" data-go=\"s6\">8.6 · Trinomials ax² + bx + c</button></div></div><div class=\"hub-card\"><h3>📝 Learning assessment</h3><p>Four short assessments built from the Chapter Check-up and the Maths Lab Activity, one per skill area. Take them in Quiz mode and open the report to see your results as pie charts.</p><div class=\"hub-btns\"><button class=\"hub-btn\" data-go=\"s7\">A · Knowing and understanding</button><button class=\"hub-btn\" data-go=\"s8\">B · Investigating patterns</button><button class=\"hub-btn\" data-go=\"s9\">C · Communicating</button><button class=\"hub-btn\" data-go=\"s10\">D · Applying mathematics in real-life contexts</button><button class=\"hub-btn primary\" data-go=\"report\">📊 View my report</button></div></div></div><section class=\"note\" id=\"nintro\"><h2>About this chapter</h2><p><b>Factorisation</b> is the reverse of multiplication: we write an algebraic expression as a <b>product of its factors</b>. For example 5a + 5b = 5(a + b) and x<sup>2</sup> + 5x + 6 = (x + 2)(x + 3).</p><p>The practice sheets contain <b>all</b> the questions of the chapter in book order. Each question starts with a tag such as <b>Example 7</b>, <b>Try This</b>, <b>Ex 8A · Q2(b)</b> or <b>Check-up · MCQ 2</b>.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Assessment</th><th>Skill area</th><th>What it checks</th></tr><tr><td>A</td><td>Knowing and understanding</td><td>HCF, taking out common factors and splitting the middle term.</td></tr><tr><td>B</td><td>Investigating patterns</td><td>Spotting perfect squares, differences of squares and sum–product pairs.</td></tr><tr><td>C</td><td>Communicating</td><td>Choosing the right method and explaining or correcting a factorisation.</td></tr><tr><td>D</td><td>Applying mathematics in real-life contexts</td><td>Prizes, shirts, lawns, tiles and gardens.</td></tr></table></div><p><b>Tools:</b> ⏱ at the top times each tab (pause or reset it). ✏️ opens a scratchpad with four pens for rough work. Tap the BM badge to go back to the home page.</p><p><b>Typing answers:</b> each blank asks for <b>one</b> factor, bracket or number (never the whole product). When two brackets are asked for, the question says which comes first; when the two numbers of a sum–product pair are asked for in one box, type them in any order separated by a comma. Type powers with ^ and leave out the × sign: <span class=\"mono\">3a^2b</span> for 3a<sup>2</sup>b, <span class=\"mono\">x-13</span> for the bracket (x − 13), <span class=\"mono\">4x^2+1</span>, <span class=\"mono\">e/3</span> for {e/3}. When the HCF is asked for, give it with a positive number in front.</p></section><section class=\"note\" id=\"n81\"><h2>8.1 Factors and HCF of monomials</h2><p class=\"lt\"><b>Objective:</b> Write the prime factorisation of numbers and the irreducible factorisation of algebraic terms, and find the HCF of monomials.</p><p>36 = 1 × 36 = 2 × 18 = 3 × 12 = 4 × 9 = 6 × 6, so 1, 2, 3, 4, 6, 9, 12, 18 and 36 are the factors of 36. Writing 36 = 2 × 2 × 3 × 3, with every factor prime, is the <b>prime factorisation</b> of 36.</p><p>Algebraic terms have factors too: 7x<sup>2</sup>y = 7 × x<sup>2</sup>y = 7x × xy = 7x<sup>2</sup> × y = …  Writing 7x<sup>2</sup>y = 7 × x × x × y, where no factor can be split further, is the <b>irreducible factorisation</b>.</p><div class=\"ex\"><div class=\"exh\">Book Example 2 · Irreducible factors</div><div class=\"exl\">3x<sup>3</sup>y = 3 × x × x × x × y<br>27p<sup>2</sup>q<sup>2</sup> = 3 × 3 × 3 × p × p × q × q<br>2x(3x + y) = 2 × x × (3x + y): here 2, x and (3x + y) are irreducible factors.</div></div><h4>Highest common factor (HCF) of monomials</h4><p><b>Method 1:</b> HCF = (HCF of the coefficients) × (each common variable with its <b>smallest</b> power). For 6xy and 8x<sup>2</sup>y: HCF(6, 8) = 2, HCF(x, x<sup>2</sup>) = x, HCF(y, y) = y, so the HCF is <b>2xy</b>.</p><p><b>Method 2:</b> write both terms as irreducible factors and multiply the factors that appear in both: 6xy = <u>2</u> × 3 × <u>x</u> × <u>y</u>, 8x<sup>2</sup>y = <u>2</u> × 2 × 2 × <u>x</u> × x × <u>y</u> → 2 × x × y = 2xy.</p><div class=\"ex\"><div class=\"exh\">Book Example 3 · 3x<sup>3</sup>y and 2xy</div><div class=\"exl\">Common factors: 1, x, y and xy.<br>HCF(3, 2) = 1, HCF(x<sup>3</sup>, x) = x, HCF(y, y) = y, so the HCF is <b>xy</b>.</div></div><div class=\"keybox\"><b>Remember:</b> a variable that is missing from one of the terms is not common (its power there is 0), so it does not appear in the HCF.</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s1\">Practise 8.1 →</button></div></section><section class=\"note\" id=\"n82\"><h2>8.2 Factorisation using the HCF</h2><p class=\"lt\"><b>Objective:</b> Factorise polynomials by taking out the highest common factor of all the terms.</p><p>35 + 25 = 7 × 5 + 5 × 5 = 5(7 + 5). In the same way 5a + 5b = 5 × a + 5 × b = <b>5(a + b)</b>: 5 and (a + b) are the factors. To factorise a polynomial, find the HCF of all its terms, divide each term by it and write the quotients in a bracket.</p><div class=\"ex\"><div class=\"exh\">Book Example 4 · 8x<sup>2</sup> − 6x<sup>2</sup>y</div><div class=\"exl\">HCF of 8 and 6 is 2; x<sup>2</sup> is common; y is not in the first term.<br>HCF = 2x<sup>2</sup>, so 8x<sup>2</sup> − 6x<sup>2</sup>y = <b>2x<sup>2</sup>(4 − 3y)</b>.</div></div><div class=\"ex\"><div class=\"exh\">Book Example 8 · 18a<sup>2</sup>b<sup>3</sup>c − 12abc + 24ab<sup>2</sup>c<sup>2</sup></div><div class=\"exl\">HCF = 6abc.<br>= 6abc × 3ab<sup>2</sup> − 6abc × 2 + 6abc × 4bc = <b>6abc(3ab<sup>2</sup> − 2 + 4bc)</b>.</div></div><div class=\"keybox\"><b>Check by expanding:</b> multiply the HCF back into the bracket; you must get the original expression. If a term equals the HCF, 1 is left in its place: 5x<sup>2</sup> − 10x<sup>3</sup> = 5x<sup>2</sup>(1 − 2x).</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s2\">Practise 8.2 →</button></div></section><section class=\"note\" id=\"n83\"><h2>8.3 Factorisation by regrouping</h2><p class=\"lt\"><b>Objective:</b> Group the terms of a polynomial so that a common binomial factor appears, then factorise.</p><p>When the terms have no common factor (other than 1), group them in pairs so that each pair has a common factor and the <b>same bracket</b> is left in both pairs. Then take that bracket out.</p><div class=\"ex\"><div class=\"exh\">Book Example 9 · 5ab − 3a + 10b − 6</div><div class=\"exl\">5ab − 3a = a(5b − 3) and 10b − 6 = 2(5b − 3).<br>So 5ab − 3a + 10b − 6 = a(5b − 3) + 2(5b − 3) = <b>(5b − 3)(a + 2)</b>.</div></div><div class=\"ex\"><div class=\"exh\">Book Example 12 · 10xy − 5y + 8 − 16x</div><div class=\"exl\">10xy − 5y = 5y(2x − 1).<br>Rewrite 8 − 16x = −16x + 8 = −8(2x − 1) to get the same bracket.<br>So the expression = 5y(2x − 1) − 8(2x − 1) = <b>(2x − 1)(5y − 8)</b>.</div></div><div class=\"keybox\"><b>Tip:</b> if the brackets do not match, try another grouping or take out a negative factor, e.g. −5x − 5 = −5(x + 1).</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s3\">Practise 8.3 →</button></div></section><section class=\"note\" id=\"n84\"><h2>8.4 Factorisation using identities</h2><p class=\"lt\"><b>Objective:</b> Use (a ± b)² and a² − b² = (a + b)(a − b) to factorise expressions and to evaluate products quickly.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Identity</th><th>Use it when you see</th></tr><tr><td>a<sup>2</sup> + 2ab + b<sup>2</sup> = (a + b)<sup>2</sup></td><td>two squares and + twice their product</td></tr><tr><td>a<sup>2</sup> − 2ab + b<sup>2</sup> = (a − b)<sup>2</sup></td><td>two squares and − twice their product</td></tr><tr><td>a<sup>2</sup> − b<sup>2</sup> = (a + b)(a − b)</td><td>a difference of two squares</td></tr></table></div><div class=\"ex\"><div class=\"exh\">Book Example 14 · x<sup>2</sup> − 6x + 9</div><div class=\"exl\">x<sup>2</sup> = (x)<sup>2</sup>, 9 = 3<sup>2</sup> and −6x = −2 × x × 3.<br>So x<sup>2</sup> − 6x + 9 = <b>(x − 3)<sup>2</sup></b> (or (3 − x)<sup>2</sup>).</div></div><div class=\"ex\"><div class=\"exh\">Book Example 16 · p<sup>2</sup> + 25q<sup>2</sup> + 10pq − 36</div><div class=\"exl\">(p<sup>2</sup> + 10pq + 25q<sup>2</sup>) − 36 = (p + 5q)<sup>2</sup> − 6<sup>2</sup><br>= <b>(p + 5q + 6)(p + 5q − 6)</b>.</div></div><div class=\"ex\"><div class=\"exh\">Book Example 18 · 703 × 697</div><div class=\"exl\">(700 + 3)(700 − 3) = 700<sup>2</sup> − 3<sup>2</sup> = 490000 − 9 = <b>489991</b>.</div></div><div class=\"ex\"><div class=\"exh\">Book Example 22 · 11p<sup>5</sup> − 44p</div><div class=\"exl\">Take out the HCF first: 11p(p<sup>4</sup> − 4).<br>p<sup>4</sup> − 4 = (p<sup>2</sup>)<sup>2</sup> − 2<sup>2</sup>, so the answer is <b>11p(p<sup>2</sup> + 2)(p<sup>2</sup> − 2)</b>.</div></div><div class=\"keybox\"><b>Always take out the HCF first</b>, then look for an identity. Keep going until no factor can be split further: x<sup>8</sup> − y<sup>8</sup> = (x<sup>4</sup> + y<sup>4</sup>)(x<sup>2</sup> + y<sup>2</sup>)(x + y)(x − y).</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s4\">Practise 8.4 →</button></div></section><section class=\"note\" id=\"n85\"><h2>8.5 Trinomials x² + ax + b</h2><p class=\"lt\"><b>Objective:</b> Factorise x² + ax + b by finding two numbers whose sum is a and product is b (splitting the middle term).</p><p>(x + 3)(x + 5) = x<sup>2</sup> + 3x + 5x + 15 = x<sup>2</sup> + 8x + 15 = x<sup>2</sup> + (sum of the constants)x + (product of the constants).</p><p>So to factorise x<sup>2</sup> + ax + b, find two numbers whose <b>sum is a</b> and whose <b>product is b</b>. Split the middle term with them, group in pairs and take out the common bracket.</p><div class=\"tscroll\"><table class=\"ttab\"><tr><th>Product b</th><th>Sum a</th><th>The two numbers</th></tr><tr><td>positive</td><td>positive</td><td>both positive</td></tr><tr><td>positive</td><td>negative</td><td>both negative</td></tr><tr><td>negative</td><td>positive</td><td>one negative; the bigger one positive</td></tr><tr><td>negative</td><td>negative</td><td>one negative; the bigger one negative</td></tr></table></div><div class=\"ex\"><div class=\"exh\">Book Example 27 · x<sup>2</sup> + 20x + 91</div><div class=\"exl\">91 = 1 × 91 = 7 × 13; 7 + 13 = 20.<br>x<sup>2</sup> + 7x + 13x + 91 = x(x + 7) + 13(x + 7) = <b>(x + 7)(x + 13)</b>.</div></div><div class=\"ex\"><div class=\"exh\">Book Example 31 · x<sup>2</sup> − 6x − 40</div><div class=\"exl\">Product −40, sum −6: the numbers are −10 and 4.<br>x<sup>2</sup> − 10x + 4x − 40 = x(x − 10) + 4(x − 10) = <b>(x − 10)(x + 4)</b>.</div></div><div class=\"ex\"><div class=\"exh\">Book Example 35 · (y − x)<sup>2</sup> − 10(x − y) + 25</div><div class=\"exl\">Let y − x = a; then x − y = −a.<br>a<sup>2</sup> + 10a + 25 = (a + 5)<sup>2</sup> = <b>(y − x + 5)<sup>2</sup></b>.</div></div><div class=\"keybox\"><b>Check:</b> multiply your two brackets; the middle term must come out right, sign included.</div><h4>Area model (Maths Lab Activity)</h4><p>Take an x × x square, strips of x × 1 and unit squares. (x + 2)(x + 3) is a rectangle x + 3 long and x + 2 wide: 1 square (x<sup>2</sup>), 3 + 2 = 5 strips (5x) and 3 × 2 = 6 unit squares, so (x + 2)(x + 3) = x<sup>2</sup> + 5x + 6. Factorising reverses this: arrange the tiles of x<sup>2</sup> + 5x + 6 into a rectangle and read off its sides.</p><div class=\"fig\"><svg class=\"figsvg\" viewBox=\"0 0 218 172\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><rect class=\"sh\" x=\"34\" y=\"26\" width=\"96\" height=\"96\"/><rect class=\"sh3\" x=\"130\" y=\"26\" width=\"18\" height=\"96\"/><rect class=\"sh3\" x=\"148\" y=\"26\" width=\"18\" height=\"96\"/><rect class=\"sh3\" x=\"166\" y=\"26\" width=\"18\" height=\"96\"/><rect class=\"sh3\" x=\"34\" y=\"122\" width=\"96\" height=\"18\"/><rect class=\"sh3\" x=\"34\" y=\"140\" width=\"96\" height=\"18\"/><rect class=\"cell\" x=\"130\" y=\"122\" width=\"18\" height=\"18\"/><rect class=\"cell\" x=\"130\" y=\"140\" width=\"18\" height=\"18\"/><rect class=\"cell\" x=\"148\" y=\"122\" width=\"18\" height=\"18\"/><rect class=\"cell\" x=\"148\" y=\"140\" width=\"18\" height=\"18\"/><rect class=\"cell\" x=\"166\" y=\"122\" width=\"18\" height=\"18\"/><rect class=\"cell\" x=\"166\" y=\"140\" width=\"18\" height=\"18\"/><text class=\"lb\" x=\"82.0\" y=\"14.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">x</text><text class=\"lb\" x=\"157.0\" y=\"14.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">3</text><text class=\"lb\" x=\"20.0\" y=\"74.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">x</text><text class=\"lb\" x=\"20.0\" y=\"140.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">2</text><line class=\"tick\" x1=\"34.0\" y1=\"22.0\" x2=\"130.0\" y2=\"22.0\"/><line class=\"tick\" x1=\"130.0\" y1=\"22.0\" x2=\"184.0\" y2=\"22.0\"/><line class=\"tick\" x1=\"30.0\" y1=\"26.0\" x2=\"30.0\" y2=\"122.0\"/><line class=\"tick\" x1=\"30.0\" y1=\"122.0\" x2=\"30.0\" y2=\"158.0\"/><text class=\"lb\" x=\"82.0\" y=\"74.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">x²</text><text class=\"lb\" x=\"139.0\" y=\"74.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">x</text><text class=\"lb\" x=\"157.0\" y=\"74.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">x</text><text class=\"lb\" x=\"175.0\" y=\"74.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">x</text><text class=\"lb\" x=\"82.0\" y=\"132.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">x</text><text class=\"lb\" x=\"82.0\" y=\"150.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">x</text></svg></div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s5\">Practise 8.5 →</button></div></section><section class=\"note\" id=\"n86\"><h2>8.6 Trinomials ax² + bx + c</h2><p class=\"lt\"><b>Objective:</b> Factorise ax² + bx + c by finding two numbers whose product is ac and sum is b.</p><p>When the coefficient of x<sup>2</sup> is not 1, the two numbers must have <b>product a × c</b> (coefficient of x<sup>2</sup> × constant) and <b>sum b</b>.</p><div class=\"ex\"><div class=\"exh\">Book Example 37 · 2x<sup>2</sup> + 13x + 20</div><div class=\"exl\">Product 2 × 20 = 40, sum 13: the numbers are 5 and 8.<br>2x<sup>2</sup> + 5x + 8x + 20 = x(2x + 5) + 4(2x + 5) = <b>(x + 4)(2x + 5)</b>.</div></div><div class=\"ex\"><div class=\"exh\">Book Example 39 · 10a<sup>2</sup> − 83a − 17</div><div class=\"exl\">Product 10 × (−17) = −170, sum −83: the numbers are −85 and 2.<br>10a<sup>2</sup> − 85a + 2a − 17 = 5a(2a − 17) + 1(2a − 17) = <b>(2a − 17)(5a + 1)</b>.</div></div><div class=\"keybox\"><b>Take out a common number first</b> if there is one: 6a<sup>2</sup> − 3ab − 3b<sup>2</sup> = 3(2a<sup>2</sup> − ab − b<sup>2</sup>) = 3(2a + b)(a − b).</div><div class=\"hub-btns\" style=\"margin-top:12px\"><button class=\"hub-btn primary\" data-go=\"s6\">Practise 8.6 →</button></div></section><section class=\"note\" id=\"nsum\"><h2>Chapter checklist</h2><ul><li>Write the prime factorisation of numbers and the irreducible factorisation of algebraic terms, and find the HCF of monomials.</li><li>Factorise polynomials by taking out the highest common factor of all the terms.</li><li>Group the terms of a polynomial so that a common binomial factor appears, then factorise.</li><li>Use (a ± b)² and a² − b² = (a + b)(a − b) to factorise expressions and to evaluate products quickly.</li><li>Factorise x² + ax + b by finding two numbers whose sum is a and product is b (splitting the middle term).</li><li>Factorise ax² + bx + c by finding two numbers whose product is ac and sum is b.</li></ul><p><b>Order of attack:</b> (1) take out the HCF; (2) look for an identity; (3) for a trinomial, split the middle term; (4) for four terms, regroup.</p><div class=\"hub-btns\" style=\"margin-top:10px\"><button class=\"hub-btn\" data-go=\"s7\">Assessment A</button><button class=\"hub-btn\" data-go=\"s8\">Assessment B</button><button class=\"hub-btn\" data-go=\"s9\">Assessment C</button><button class=\"hub-btn\" data-go=\"s10\">Assessment D</button><button class=\"hub-btn primary\" data-go=\"report\">📊 Report</button></div></section>";
var REPORT = [["s7", "A", "Knowing and understanding"], ["s8", "B", "Investigating patterns"], ["s9", "C", "Communicating"], ["s10", "D", "Applying mathematics in real-life contexts"]];
var SECTIONS = [{"id": "s1", "label": "8.1 Factors & HCF", "sub": "Factors of numbers and algebraic terms, irreducible factorisation and HCF of monomials", "slides": [{"kind": "blank", "p": "<b>Looking Back · Q1</b> · State the prime factorisation of 96. Type powers with ^ and × between factors (e.g. 2³×5).", "tag": "", "marks": "", "flat": [{"t": "96 = __B1__", "a": {"B1": "2^5×3"}, "expr": "primes"}], "sol": "96 = 2 × 48 = 2 × 2 × 24 = 2 × 2 × 2 × 12 = 2 × 2 × 2 × 2 × 6 = 2 × 2 × 2 × 2 × 2 × 3 = 2<sup>5</sup> × 3."}, {"kind": "mcq", "text": "<b>Looking Back · Q2(a)</b> · Which list gives <b>all</b> the factors of 3a?", "opts": ["1, 3, a, 3a and a<sup>2</sup>", "1, 3, a and 3a", "3 and a only", "1 and 3a only"], "correct": 1, "tag": "", "sol": "3a = 1 × 3a = 3 × a, so its factors are 1, 3, a and 3a. a<sup>2</sup> does not divide 3a."}, {"kind": "blank", "p": "<b>Looking Back · Q2(b)</b> · List the factors of xyz.", "tag": "", "marks": "", "flat": [{"t": "Counting 1 and xyz itself, xyz has __B1__ factors.", "a": {"B1": "8"}}], "sol": "The factors are 1, x, y, z, xy, yz, zx and xyz: 8 factors."}, {"kind": "mcq", "text": "<b>Looking Back · Q2(c)</b> · The factors of the terms of 2a<sup>2</sup>b − 5 are", "opts": ["2a<sup>2</sup>b = 2 × a × b and −5 = −1 × 5", "2a<sup>2</sup>b − 5 = (2a<sup>2</sup> − 5) × b", "2a<sup>2</sup>b = 2 × a × a × b and −5 = −1 × 5", "2a<sup>2</sup>b = 2 × 2 × a × b and −5 = 5 × 1"], "correct": 2, "tag": "", "sol": "Each term is split separately: 2a<sup>2</sup>b = 2 × a × a × b (a<sup>2</sup> = a × a) and −5 = −1 × 5."}, {"kind": "blank", "p": "<b>Looking Back · Q2(d)</b> · List the factors of each term of 5xy − 10x<sup>2</sup>y.", "tag": "", "marks": "", "flat": [{"t": "5xy = 5 × x × __B1__", "a": {"B1": "y"}, "expr": true}, {"t": "−10x<sup>2</sup>y = −1 × 2 × 5 × x × __B1__ × y", "a": {"B1": "x"}, "expr": true}], "sol": "5xy = 5 × x × y.\n10 = 2 × 5 and x<sup>2</sup> = x × x, so −10x<sup>2</sup>y = −1 × 2 × 5 × x × x × y."}, {"kind": "mcq", "text": "<b>Looking Back · Q2(e)</b> · The factors of the terms of ax + by − 72 are", "opts": ["a, x;  b, y;  −1, 2, 2, 3, 3, 3", "ax;  by;  −72 (no smaller factors)", "a, x;  b, y;  −1, 2, 3, 12", "a, x;  b, y;  −1, 2, 2, 2, 3, 3"], "correct": 3, "tag": "", "sol": "ax = a × x, by = b × y and −72 = −1 × 2 × 2 × 2 × 3 × 3 (2<sup>3</sup> × 3<sup>2</sup> = 72). 2 × 2 × 3 × 3 × 3 = 108, and 12 is not prime."}, {"kind": "blank", "p": "<b>Looking Back · Q3</b> · Solve (add the like terms).", "tag": "", "marks": "", "flat": [{"t": "a) 5x<sup>2</sup> + 8x<sup>2</sup> + 15x<sup>2</sup> − 18x<sup>2</sup> = __B1__", "a": {"B1": "10x^2"}, "expr": true}, {"t": "b) 3y<sup>3</sup> + 4y<sup>3</sup> − 7y<sup>3</sup> + 3y<sup>3</sup> − y<sup>2</sup> = __B1__", "a": {"B1": "3y^3-y^2"}, "expr": true}, {"t": "c) ab<sup>2</sup> + 4a<sup>2</sup>b − 3a<sup>2</sup>b + ab = __B1__", "a": {"B1": "ab^2+a^2b+ab"}, "expr": true}], "sol": "(5 + 8 + 15 − 18)x<sup>2</sup> = 10x<sup>2</sup>.\n(3 + 4 − 7 + 3)y<sup>3</sup> − y<sup>2</sup> = 3y<sup>3</sup> − y<sup>2</sup> (y<sup>2</sup> is not a like term).\n4a<sup>2</sup>b − 3a<sup>2</sup>b = a<sup>2</sup>b; ab<sup>2</sup> and ab stay: ab<sup>2</sup> + a<sup>2</sup>b + ab."}, {"kind": "blank", "p": "<b>Looking Back · Q4</b> · Simplify.", "tag": "", "marks": "", "flat": [{"t": "a) 6 − {4 − (p − 8)} = __B1__", "a": {"B1": "p-6"}, "expr": true}, {"t": "b) 11 − [2 + {3 − (x − 9)}] = __B1__", "a": {"B1": "x-3"}, "expr": true}], "sol": "4 − (p − 8) = 12 − p; 6 − (12 − p) = p − 6.\n3 − (x − 9) = 12 − x; 2 + 12 − x = 14 − x; 11 − (14 − x) = x − 3."}, {"kind": "mcq", "text": "<b>Looking Back · Q5</b> · The value of 8x<sup>2</sup> + 11x + 20 when x = −3 is", "opts": ["−85", "125", "19", "59"], "correct": 3, "tag": "", "sol": "8 × (−3)<sup>2</sup> + 11 × (−3) + 20 = 72 − 33 + 20 = 59."}, {"kind": "blank", "p": "<b>Example 1</b> · State the prime factorisation of the following numbers (e.g. 2³×5).", "tag": "", "marks": "", "flat": [{"t": "a) 60 = __B1__", "a": {"B1": "2^2×3×5"}, "expr": "primes"}, {"t": "b) 18 = __B1__", "a": {"B1": "2×3^2"}, "expr": "primes"}, {"t": "c) 84 = __B1__", "a": {"B1": "2^2×3×7"}, "expr": "primes"}], "sol": "60 = 2 × 2 × 3 × 5.\n18 = 2 × 3 × 3.\n84 = 2 × 2 × 3 × 7."}, {"kind": "mcq", "text": "<b>Example 2(a)</b> · The irreducible factorisation of 3x<sup>3</sup>y is", "opts": ["3 × x<sup>3</sup> × y", "3 × 3 × x × y", "3x × x<sup>2</sup> × y", "3 × x × x × x × y"], "correct": 3, "tag": "", "sol": "Split every factor as far as it goes: x<sup>3</sup> = x × x × x, so 3x<sup>3</sup>y = 3 × x × x × x × y."}, {"kind": "blank", "p": "<b>Example 2(b, c)</b> · State the irreducible factorisation. Give the number part as primes (e.g. 2³×5).", "tag": "", "marks": "", "flat": [{"t": "b) 27p<sup>2</sup>q<sup>2</sup> = (__B1__) × p × p × q × q", "a": {"B1": "3^3"}, "expr": "primes"}, {"t": "c) 42pqr = (__B1__) × p × q × r", "a": {"B1": "2×3×7"}, "expr": "primes"}], "sol": "27 = 3 × 3 × 3, so 27p<sup>2</sup>q<sup>2</sup> = 3 × 3 × 3 × p × p × q × q.\n42 = 2 × 3 × 7, so 42pqr = 2 × 3 × 7 × p × q × r."}, {"kind": "mcq", "text": "<b>Example 2(d)</b> · The irreducible factors of 2x(3x + y) are", "opts": ["2, 3, x, x and y", "2, x, 3x and y", "2, x and (3x + y)", "2x and (3x + y) only"], "correct": 2, "tag": "", "sol": "2x(3x + y) = 2 × x × (3x + y). The bracket 3x + y cannot be split into factors, so it is itself an irreducible factor."}, {"kind": "blank", "p": "<b>Example 3</b> · Find the common factors of 3x<sup>3</sup>y and 2xy, and their HCF.", "tag": "", "marks": "", "flat": [{"t": "HCF of the coefficients 3 and 2 = __B1__", "a": {"B1": "1"}}, {"t": "Common factors: 1, x, y and __B1__", "a": {"B1": "xy"}, "expr": true}, {"t": "HCF = __B1__", "a": {"B1": "xy"}, "expr": true}], "sol": "3 and 2 have no common factor except 1.\nFactors of 2xy are 1, 2, x, 2x, y, 2y, xy, 2xy; of these 1, x, y and xy also divide 3x<sup>3</sup>y.\nThe highest of them is xy."}, {"kind": "blank", "p": "<b>Try This (p. 98) · (a–d)</b> · Find the highest common factor of:", "tag": "", "marks": "", "flat": [{"t": "a) x<sup>3</sup>, x: HCF = __B1__", "a": {"B1": "x"}, "expr": true}, {"t": "b) x<sup>2</sup>y, xy: HCF = __B1__", "a": {"B1": "xy"}, "expr": true}, {"t": "c) 8x<sup>2</sup>y<sup>2</sup>, 4x<sup>2</sup>y: HCF = __B1__", "a": {"B1": "4x^2y"}, "expr": true}, {"t": "d) x<sup>3</sup>y, x<sup>2</sup>: HCF = __B1__", "a": {"B1": "x^2"}, "expr": true}], "sol": "a) HCF of the coefficients 1, 1 is 1; common variables with the smallest powers: x. HCF = x.\nb) HCF of the coefficients 1, 1 is 1; common variables with the smallest powers: x, y. HCF = xy.\nc) HCF of the coefficients 8, 4 is 4; common variables with the smallest powers: x<sup>2</sup>, y. HCF = 4x<sup>2</sup>y.\nd) HCF of the coefficients 1, 1 is 1; common variables with the smallest powers: x<sup>2</sup>. HCF = x<sup>2</sup>."}, {"kind": "mcq", "text": "The HCF of 12a<sup>3</sup>b and 18a<sup>2</sup>b<sup>4</sup> is", "opts": ["6a<sup>3</sup>b<sup>4</sup>", "6ab", "6a<sup>2</sup>b", "36a<sup>3</sup>b<sup>4</sup>"], "correct": 2, "tag": "", "sol": "HCF(12, 18) = 6; take each common variable with its <b>smallest</b> power: a<sup>2</sup> and b. HCF = 6a<sup>2</sup>b."}, {"kind": "blank", "p": "<b>Try This (p. 98) · (e–h)</b> · Find the highest common factor of:", "tag": "", "marks": "", "flat": [{"t": "e) 7xy, 5x<sup>3</sup>: HCF = __B1__", "a": {"B1": "x"}, "expr": true}, {"t": "f) 9x<sup>3</sup>y, 18xy<sup>2</sup>: HCF = __B1__", "a": {"B1": "9xy"}, "expr": true}, {"t": "g) x<sup>2</sup>y, xy, 5x<sup>2</sup>y<sup>2</sup>: HCF = __B1__", "a": {"B1": "xy"}, "expr": true}, {"t": "h) 20x<sup>3</sup>y<sup>2</sup>, 40xy<sup>2</sup>, 60x: HCF = __B1__", "a": {"B1": "20x"}, "expr": true}], "sol": "e) HCF of the coefficients 7, 5 is 1; common variables with the smallest powers: x. HCF = x.\nf) HCF of the coefficients 9, 18 is 9; common variables with the smallest powers: x, y. HCF = 9xy.\ng) HCF of the coefficients 1, 1, 5 is 1; common variables with the smallest powers: x, y. HCF = xy.\nh) HCF of the coefficients 20, 40, 60 is 20; common variables with the smallest powers: x. HCF = 20x."}, {"kind": "blank", "p": "<b>Ex 8A · Q1(a–e)</b> · Find the HCF of the following terms.", "tag": "", "marks": "", "flat": [{"t": "a) 12x<sup>2</sup>, 16y<sup>3</sup>: HCF = __B1__", "a": {"B1": "4"}, "expr": true}, {"t": "b) 15a<sup>2</sup>b<sup>2</sup>, −24ab: HCF = __B1__", "a": {"B1": "3ab"}, "expr": true}, {"t": "c) 18x<sup>3</sup>y, 27x<sup>2</sup>y<sup>2</sup>z: HCF = __B1__", "a": {"B1": "9x^2y"}, "expr": true}, {"t": "d) 72abc, 27a<sup>2</sup>bc: HCF = __B1__", "a": {"B1": "9abc"}, "expr": true}, {"t": "e) 71x<sup>2</sup>y<sup>2</sup>z<sup>3</sup>, 81x<sup>2</sup>y<sup>2</sup>z<sup>3</sup>: HCF = __B1__", "a": {"B1": "x^2y^2z^3"}, "expr": true}], "sol": "a) HCF of the coefficients 12, 16 is 4; no variable is common to all the terms. HCF = 4.\nb) HCF of the coefficients 15, 24 is 3; common variables with the smallest powers: a, b. HCF = 3ab.\nc) HCF of the coefficients 18, 27 is 9; common variables with the smallest powers: x<sup>2</sup>, y. HCF = 9x<sup>2</sup>y.\nd) HCF of the coefficients 72, 27 is 9; common variables with the smallest powers: a, b, c. HCF = 9abc.\ne) HCF of the coefficients 71, 81 is 1; common variables with the smallest powers: x<sup>2</sup>, y<sup>2</sup>, z<sup>3</sup>. HCF = x<sup>2</sup>y<sup>2</sup>z<sup>3</sup>."}, {"kind": "mcq", "text": "<b>Ex 8A · Q1(f)</b> · The HCF of 3xyz and 27 is", "opts": ["3xyz", "27", "3", "1"], "correct": 2, "tag": "", "sol": "HCF(3, 27) = 3; 27 has no variables, so no variable is common. HCF = 3."}, {"kind": "blank", "p": "<b>Ex 8A · Q1(g–i)</b> · Find the HCF of the following terms.", "tag": "", "marks": "", "flat": [{"t": "g) 39a<sup>3</sup>b<sup>3</sup>c<sup>3</sup>, 26a<sup>2</sup>b<sup>2</sup>c<sup>2</sup>: HCF = __B1__", "a": {"B1": "13a^2b^2c^2"}, "expr": true}, {"t": "h) 2ab, 7ac, 9bc: HCF = __B1__", "a": {"B1": "1"}, "expr": true}, {"t": "i) 36a<sup>2</sup>b<sup>2</sup>c<sup>2</sup>, 27a<sup>2</sup>b<sup>2</sup>c<sup>2</sup>, 72abc: HCF = __B1__", "a": {"B1": "9abc"}, "expr": true}], "sol": "g) HCF of the coefficients 39, 26 is 13; common variables with the smallest powers: a<sup>2</sup>, b<sup>2</sup>, c<sup>2</sup>. HCF = 13a<sup>2</sup>b<sup>2</sup>c<sup>2</sup>.\nh) HCF of the coefficients 2, 7, 9 is 1; no variable is common to all the terms. HCF = 1.\ni) HCF of the coefficients 36, 27, 72 is 9; common variables with the smallest powers: a, b, c. HCF = 9abc."}]}, {"id": "s2", "label": "8.2 Taking out HCF", "sub": "Factorising polynomials by taking out the highest common factor", "slides": [{"kind": "blank", "p": "<b>Example 4</b> · Factorise 8x<sup>2</sup> − 6x<sup>2</sup>y. Take out the HCF (first blank: the HCF, with a positive coefficient; second blank: what is left in the bracket).", "tag": "", "marks": "", "flat": [{"t": "8x<sup>2</sup> − 6x<sup>2</sup>y = __B1__(__B2__)", "a": {"B1": "2x^2", "B2": "4-3y"}, "expr": true}], "sol": "8x<sup>2</sup> = 2 × 2 × 2 × x × x and 6x<sup>2</sup>y = 2 × 3 × x × x × y, so the HCF is 2x<sup>2</sup> (y is not common). 8x<sup>2</sup> − 6x<sup>2</sup>y = 2x<sup>2</sup>(4 − 3y)."}, {"kind": "mcq", "text": "<b>Example 5</b> · 72x<sup>4</sup>y<sup>3</sup> − 54x<sup>5</sup>y<sup>2</sup> factorised by taking out the HCF is", "opts": ["18x<sup>4</sup>y<sup>2</sup>(4y − 3x)", "18x<sup>4</sup>y<sup>2</sup>(4y + 3x)", "9x<sup>4</sup>y<sup>2</sup>(8y − 3x)", "18x<sup>4</sup>y<sup>2</sup>(3x − 4y)"], "correct": 0, "tag": "", "sol": "In irreducible factors the common ones are 2 × 3 × 3 × x × x × x × x × y × y = 18x<sup>4</sup>y<sup>2</sup>. 72x<sup>4</sup>y<sup>3</sup> ÷ 18x<sup>4</sup>y<sup>2</sup> = 4y and 54x<sup>5</sup>y<sup>2</sup> ÷ 18x<sup>4</sup>y<sup>2</sup> = 3x, so the answer is 18x<sup>4</sup>y<sup>2</sup>(4y − 3x)."}, {"kind": "blank", "p": "<b>Example 6</b> · Factorise 12x<sup>3</sup>y<sup>2</sup>z + 8x<sup>2</sup>y<sup>2</sup>z<sup>2</sup>. Take out the HCF (first blank: the HCF, with a positive coefficient; second blank: what is left in the bracket).", "tag": "", "marks": "", "flat": [{"t": "12x<sup>3</sup>y<sup>2</sup>z + 8x<sup>2</sup>y<sup>2</sup>z<sup>2</sup> = __B1__(__B2__)", "a": {"B1": "4x^2y^2z", "B2": "3x+2z"}, "expr": true}], "sol": "HCF of 12 and 8 is 4; of x<sup>3</sup> and x<sup>2</sup> is x<sup>2</sup>; of y<sup>2</sup> and y<sup>2</sup> is y<sup>2</sup>; of z and z<sup>2</sup> is z. HCF = 4x<sup>2</sup>y<sup>2</sup>z, and the expression = 4x<sup>2</sup>y<sup>2</sup>z(3x + 2z)."}, {"kind": "mcq", "text": "<b>Example 7</b> · 25x<sup>2</sup>y<sup>3</sup> − 10xy<sup>2</sup>z<sup>2</sup> factorised by taking out the HCF is", "opts": ["5x<sup>2</sup>y<sup>2</sup>(5y − 2z<sup>2</sup>)", "5xy<sup>2</sup>(5xy + 2z<sup>2</sup>)", "5xy<sup>2</sup>(5xy − 2z<sup>2</sup>)", "5xy<sup>2</sup>(5x − 2z<sup>2</sup>)"], "correct": 2, "tag": "", "sol": "HCF of 25 and 10 is 5, of x<sup>2</sup> and x is x, of y<sup>3</sup> and y<sup>2</sup> is y<sup>2</sup>; z is not in the first term. HCF = 5xy<sup>2</sup>, so the answer is 5xy<sup>2</sup>(5xy − 2z<sup>2</sup>)."}, {"kind": "blank", "p": "<b>Example 8</b> · Factorise 18a<sup>2</sup>b<sup>3</sup>c − 12abc + 24ab<sup>2</sup>c<sup>2</sup>. Take out the HCF (first blank: the HCF, with a positive coefficient; second blank: what is left in the bracket).", "tag": "", "marks": "", "flat": [{"t": "18a<sup>2</sup>b<sup>3</sup>c − 12abc + 24ab<sup>2</sup>c<sup>2</sup> = __B1__(__B2__)", "a": {"B1": "6abc", "B2": "3ab^2-2+4bc"}, "expr": true}], "sol": "HCF = 6abc. 18a<sup>2</sup>b<sup>3</sup>c ÷ 6abc = 3ab<sup>2</sup>, 12abc ÷ 6abc = 2, 24ab<sup>2</sup>c<sup>2</sup> ÷ 6abc = 4bc, so the answer is 6abc(3ab<sup>2</sup> − 2 + 4bc)."}, {"kind": "blank", "p": "<b>Try This (p. 99)</b> · Factorise. Take out the HCF (first blank: the HCF, with a positive coefficient; second blank: what is left in the bracket).", "tag": "", "marks": "", "flat": [{"t": "a) 25x<sup>3</sup>yz − 65xy<sup>3</sup> = __B1__(__B2__)", "a": {"B1": "5xy", "B2": "5x^2z-13y^2"}, "expr": true}, {"t": "b) 8ab<sup>4</sup>c<sup>2</sup> + 44a<sup>2</sup>b<sup>3</sup>c = __B1__(__B2__)", "a": {"B1": "4ab^3c", "B2": "2bc+11a"}, "expr": true}], "sol": "a) HCF of the terms = 5xy. Divide each term by 5xy: 25x<sup>3</sup>yz − 65xy<sup>3</sup> = 5xy(5x<sup>2</sup>z − 13y<sup>2</sup>).\nb) HCF of the terms = 4ab<sup>3</sup>c. Divide each term by 4ab<sup>3</sup>c: 8ab<sup>4</sup>c<sup>2</sup> + 44a<sup>2</sup>b<sup>3</sup>c = 4ab<sup>3</sup>c(2bc + 11a)."}, {"kind": "mcq", "text": "<b>Ex 8A · Q2(a)</b> · 6a + 6b factorised is", "opts": ["6(a + b)", "6a(1 + b)", "a(6 + 6b)", "6(a + 6b)"], "correct": 0, "tag": "", "sol": "6 is common to both terms: 6a + 6b = 6(a + b)."}, {"kind": "blank", "p": "<b>Ex 8A · Q2(b–f)</b> · Factorise the following. Take out the HCF (first blank: the HCF, with a positive coefficient; second blank: what is left in the bracket).", "tag": "", "marks": "", "flat": [{"t": "b) {1/2}x + {1/2} = __B1__(__B2__)", "a": {"B1": "1/2", "B2": "x+1"}, "expr": true}, {"t": "c) ax + bx = __B1__(__B2__)", "a": {"B1": "x", "B2": "a+b"}, "expr": true}, {"t": "d) cdm + cdt = __B1__(__B2__)", "a": {"B1": "cd", "B2": "m+t"}, "expr": true}, {"t": "e) 28x − 14y = __B1__(__B2__)", "a": {"B1": "14", "B2": "2x-y"}, "expr": true}, {"t": "f) 4xy + 4y = __B1__(__B2__)", "a": {"B1": "4y", "B2": "x+1"}, "expr": true}], "sol": "b) {1/2} is common: {1/2}x + {1/2} = {1/2}(x + 1).\nc) HCF of the terms = x. Divide each term by x: ax + bx = x(a + b).\nd) HCF of the terms = cd. Divide each term by cd: cdm + cdt = cd(m + t).\ne) HCF of the terms = 14. Divide each term by 14: 28x − 14y = 14(2x − y).\nf) HCF of the terms = 4y. Divide each term by 4y: 4xy + 4y = 4y(x + 1). (4y ÷ 4y = 1.)"}, {"kind": "mcq", "text": "<b>Ex 8A · Q2(g)</b> · 3x<sup>2</sup> − 6a<sup>6</sup> factorised is", "opts": ["6(x<sup>2</sup> − a<sup>6</sup>)", "3x<sup>2</sup>(1 − 2a<sup>6</sup>)", "3(x<sup>2</sup> − 2a<sup>3</sup>)", "3(x<sup>2</sup> − 2a<sup>6</sup>)"], "correct": 3, "tag": "", "sol": "The only common factor is 3 (x and a are not in both terms): 3(x<sup>2</sup> − 2a<sup>6</sup>)."}, {"kind": "blank", "p": "<b>Ex 8A · Q2(h–l)</b> · Factorise the following. Take out the HCF (first blank: the HCF, with a positive coefficient; second blank: what is left in the bracket).", "tag": "", "marks": "", "flat": [{"t": "h) 36a<sup>2</sup>b<sup>3</sup> − 18a<sup>3</sup>b<sup>2</sup> = __B1__(__B2__)", "a": {"B1": "18a^2b^2", "B2": "2b-a"}, "expr": true}, {"t": "i) 2xy<sup>2</sup> + 2xy = __B1__(__B2__)", "a": {"B1": "2xy", "B2": "y+1"}, "expr": true}, {"t": "j) 9x<sup>2</sup> + 3x = __B1__(__B2__)", "a": {"B1": "3x", "B2": "3x+1"}, "expr": true}, {"t": "k) 25m<sup>2</sup>n<sup>3</sup> − 5mn = __B1__(__B2__)", "a": {"B1": "5mn", "B2": "5mn^2-1"}, "expr": true}, {"t": "l) 12y<sup>3</sup> + 6a<sup>3</sup> = __B1__(__B2__)", "a": {"B1": "6", "B2": "2y^3+a^3"}, "expr": true}], "sol": "h) HCF of the terms = 18a<sup>2</sup>b<sup>2</sup>. Divide each term by 18a<sup>2</sup>b<sup>2</sup>: 36a<sup>2</sup>b<sup>3</sup> − 18a<sup>3</sup>b<sup>2</sup> = 18a<sup>2</sup>b<sup>2</sup>(2b − a).\ni) HCF of the terms = 2xy. Divide each term by 2xy: 2xy<sup>2</sup> + 2xy = 2xy(y + 1).\nj) HCF of the terms = 3x. Divide each term by 3x: 9x<sup>2</sup> + 3x = 3x(3x + 1).\nk) HCF of the terms = 5mn. Divide each term by 5mn: 25m<sup>2</sup>n<sup>3</sup> − 5mn = 5mn(5mn<sup>2</sup> − 1).\nl) HCF of the terms = 6. Divide each term by 6: 12y<sup>3</sup> + 6a<sup>3</sup> = 6(2y<sup>3</sup> + a<sup>3</sup>)."}, {"kind": "mcq", "text": "<b>Ex 8A · Q2(m)</b> · 12x<sup>3</sup>y − 4xy<sup>2</sup> factorised is", "opts": ["12xy(x<sup>2</sup> − 4y)", "4xy(3x<sup>2</sup> − y<sup>2</sup>)", "4xy(3x<sup>2</sup> − y)", "4x<sup>2</sup>y(3x − y)"], "correct": 2, "tag": "", "sol": "HCF = 4xy. 12x<sup>3</sup>y ÷ 4xy = 3x<sup>2</sup> and 4xy<sup>2</sup> ÷ 4xy = y: 4xy(3x<sup>2</sup> − y)."}, {"kind": "blank", "p": "<b>Ex 8A · Q2(n–q)</b> · Factorise the following. Take out the HCF (first blank: the HCF, with a positive coefficient; second blank: what is left in the bracket).", "tag": "", "marks": "", "flat": [{"t": "n) 3ay + 3az = __B1__(__B2__)", "a": {"B1": "3a", "B2": "y+z"}, "expr": true}, {"t": "o) 9x − 12 = __B1__(__B2__)", "a": {"B1": "3", "B2": "3x-4"}, "expr": true}, {"t": "p) 10x − 25x<sup>2</sup> = __B1__(__B2__)", "a": {"B1": "5x", "B2": "2-5x"}, "expr": true}, {"t": "q) 3x<sup>2</sup>y + xy = __B1__(__B2__)", "a": {"B1": "xy", "B2": "3x+1"}, "expr": true}], "sol": "n) HCF of the terms = 3a. Divide each term by 3a: 3ay + 3az = 3a(y + z).\no) HCF of the terms = 3. Divide each term by 3: 9x − 12 = 3(3x − 4).\np) HCF of the terms = 5x. Divide each term by 5x: 10x − 25x<sup>2</sup> = 5x(2 − 5x).\nq) HCF of the terms = xy. Divide each term by xy: 3x<sup>2</sup>y + xy = xy(3x + 1)."}, {"kind": "mcq", "text": "<b>Ex 8A · Q2(r)</b> · 21a<sup>2</sup>b + 49ab<sup>2</sup> factorised is", "opts": ["7ab(3a + 7b)", "21ab(a + 2b)", "7a(3ab + 7b)", "7ab(3b + 7a)"], "correct": 0, "tag": "", "sol": "HCF = 7ab. 21a<sup>2</sup>b ÷ 7ab = 3a and 49ab<sup>2</sup> ÷ 7ab = 7b: 7ab(3a + 7b)."}, {"kind": "blank", "p": "<b>Ex 8A · Q2(s, t)</b> · Factorise the following. Take out the HCF (first blank: the HCF, with a positive coefficient; second blank: what is left in the bracket).", "tag": "", "marks": "", "flat": [{"t": "s) 7a<sup>2</sup>b − 14b<sup>2</sup> = __B1__(__B2__)", "a": {"B1": "7b", "B2": "a^2-2b"}, "expr": true}, {"t": "t) −16a<sup>2</sup> + 20a<sup>3</sup> = __B1__(__B2__)", "a": {"B1": "4a^2", "B2": "5a-4"}, "expr": true}], "sol": "s) HCF of the terms = 7b. Divide each term by 7b: 7a<sup>2</sup>b − 14b<sup>2</sup> = 7b(a<sup>2</sup> − 2b).\nt) HCF of the terms = 4a<sup>2</sup>. Divide each term by 4a<sup>2</sup>: −16a<sup>2</sup> + 20a<sup>3</sup> = 4a<sup>2</sup>(5a − 4)."}, {"kind": "blank", "p": "<b>Ex 8A · Q3(a–c)</b> · Factorise using HCF. Take out the HCF (first blank: the HCF, with a positive coefficient; second blank: what is left in the bracket).", "tag": "", "marks": "", "flat": [{"t": "a) 5x<sup>2</sup> − 10x<sup>3</sup> + 20x<sup>4</sup> = __B1__(__B2__)", "a": {"B1": "5x^2", "B2": "1-2x+4x^2"}, "expr": true}, {"t": "b) 3x<sup>2</sup>y<sup>2</sup> − 12xy + 27x<sup>3</sup>y<sup>3</sup> = __B1__(__B2__)", "a": {"B1": "3xy", "B2": "xy-4+9x^2y^2"}, "expr": true}, {"t": "c) −10a<sup>3</sup>b + 30ab<sup>3</sup> − 20a<sup>3</sup>b<sup>3</sup> = __B1__(__B2__)", "a": {"B1": "10ab", "B2": "-a^2+3b^2-2a^2b^2"}, "expr": true}], "sol": "a) HCF of the terms = 5x<sup>2</sup>. Divide each term by 5x<sup>2</sup>: 5x<sup>2</sup> − 10x<sup>3</sup> + 20x<sup>4</sup> = 5x<sup>2</sup>(1 − 2x + 4x<sup>2</sup>).\nb) HCF of the terms = 3xy. Divide each term by 3xy: 3x<sup>2</sup>y<sup>2</sup> − 12xy + 27x<sup>3</sup>y<sup>3</sup> = 3xy(xy − 4 + 9x<sup>2</sup>y<sup>2</sup>).\nc) HCF of the terms = 10ab. Divide each term by 10ab: −10a<sup>3</sup>b + 30ab<sup>3</sup> − 20a<sup>3</sup>b<sup>3</sup> = 10ab(−a<sup>2</sup> + 3b<sup>2</sup> − 2a<sup>2</sup>b<sup>2</sup>)."}, {"kind": "mcq", "text": "<b>Ex 8A · Q3(d)</b> · 10x<sup>3</sup> − 15y<sup>3</sup> + 20z<sup>3</sup> factorised using the HCF is", "opts": ["5x<sup>3</sup>(2 − 3y<sup>3</sup> + 4z<sup>3</sup>)", "5(2x<sup>3</sup> + 3y<sup>3</sup> + 4z<sup>3</sup>)", "5(2x<sup>3</sup> − 3y<sup>3</sup> + 4z<sup>3</sup>)", "5(2x<sup>3</sup> − 3y<sup>3</sup> − 4z<sup>3</sup>)"], "correct": 2, "tag": "", "sol": "Only 5 is common to 10, 15 and 20 (no variable is in all three terms): 5(2x<sup>3</sup> − 3y<sup>3</sup> + 4z<sup>3</sup>)."}, {"kind": "blank", "p": "<b>Ex 8A · Q3(e, f)</b> · Factorise using HCF. Take out the HCF (first blank: the HCF, with a positive coefficient; second blank: what is left in the bracket).", "tag": "", "marks": "", "flat": [{"t": "e) x<sup>2</sup>yz − 4xy<sup>3</sup> + 20x<sup>3</sup> = __B1__(__B2__)", "a": {"B1": "x", "B2": "xyz-4y^3+20x^2"}, "expr": true}, {"t": "f) 4x<sup>2</sup> − 12x<sup>2</sup>y + 16x<sup>2</sup>y<sup>3</sup> = __B1__(__B2__)", "a": {"B1": "4x^2", "B2": "1-3y+4y^3"}, "expr": true}], "sol": "e) HCF of the terms = x. Divide each term by x: x<sup>2</sup>yz − 4xy<sup>3</sup> + 20x<sup>3</sup> = x(xyz − 4y<sup>3</sup> + 20x<sup>2</sup>).\nf) HCF of the terms = 4x<sup>2</sup>. Divide each term by 4x<sup>2</sup>: 4x<sup>2</sup> − 12x<sup>2</sup>y + 16x<sup>2</sup>y<sup>3</sup> = 4x<sup>2</sup>(1 − 3y + 4y<sup>3</sup>)."}, {"kind": "mcq", "text": "<b>Ex 8A · Q3(g)</b> · 5a + 5b + 5c factorised is", "opts": ["5(a + b + 5c)", "5(a + b + c)", "5(a + b) + c", "5abc"], "correct": 1, "tag": "", "sol": "5 is common to all three terms: 5(a + b + c)."}, {"kind": "blank", "p": "<b>Ex 8A · Q3(h, i)</b> · Factorise using HCF. Take out the HCF (first blank: the HCF, with a positive coefficient; second blank: what is left in the bracket).", "tag": "", "marks": "", "flat": [{"t": "h) abc + aby + abz = __B1__(__B2__)", "a": {"B1": "ab", "B2": "c+y+z"}, "expr": true}, {"t": "i) 5x + 5y − 5z = __B1__(__B2__)", "a": {"B1": "5", "B2": "x+y-z"}, "expr": true}], "sol": "h) HCF of the terms = ab. Divide each term by ab: abc + aby + abz = ab(c + y + z).\ni) HCF of the terms = 5. Divide each term by 5: 5x + 5y − 5z = 5(x + y − z)."}, {"kind": "blank", "p": "<b>Ex 8A · Q4</b> · The apples and bananas collected in an orchard on a particular day are 2a<sup>3</sup>b<sup>2</sup> and 4a<sup>4</sup>b, respectively. If they have to be packed separately in boxes which contain equal number of fruits, what is the largest number of fruits that can be packed in each box so that all the fruits are packed?", "tag": "", "marks": "", "flat": [{"t": "Largest number of fruits in each box = __B1__", "a": {"B1": "2a^3b"}, "expr": true}], "sol": "The number in a box must divide both 2a<sup>3</sup>b<sup>2</sup> and 4a<sup>4</sup>b, and be as large as possible, so it is their HCF: HCF(2, 4) = 2, a<sup>3</sup>, b → 2a<sup>3</sup>b fruits."}]}, {"id": "s3", "label": "8.3 Regrouping", "sub": "Factorisation by regrouping the terms", "slides": [{"kind": "blank", "p": "<b>Example 9</b> · Factorise 5ab − 3a + 10b − 6.", "tag": "", "marks": "", "flat": [{"t": "5ab − 3a = a(__B1__)", "a": {"B1": "5b-3"}, "expr": true}, {"t": "10b − 6 = 2(__B1__)", "a": {"B1": "5b-3"}, "expr": true}, {"t": "5ab − 3a + 10b − 6 = (5b − 3)(__B1__)", "a": {"B1": "a+2"}, "expr": true}], "sol": "5ab − 3a = a(5b − 3).\n10b − 6 = 2(5b − 3).\nNow (5b − 3) is common: a(5b − 3) + 2(5b − 3) = (5b − 3)(a + 2)."}, {"kind": "mcq", "text": "<b>Example 10</b> · 28x − 21y + 8x<sup>2</sup> − 6xy factorised is", "opts": ["(4x − 3y)(7 − 2x)", "(4x − 3y)(7 + 2x)", "(4x + 3y)(7 + 2x)", "(4x − 3y)(7x + 2)"], "correct": 1, "tag": "", "sol": "(28x − 21y) + (8x<sup>2</sup> − 6xy) = 7(4x − 3y) + 2x(4x − 3y) = (4x − 3y)(7 + 2x)."}, {"kind": "blank", "p": "<b>Example 11 · Method 2</b> · Factorise 8xy − 5x + 8y − 5 by grouping the first and third terms, and the second and last terms.", "tag": "", "marks": "", "flat": [{"t": "8xy + 8y = 8y(__B1__)", "a": {"B1": "x+1"}, "expr": true}, {"t": "−5x − 5 = −5(__B1__)", "a": {"B1": "x+1"}, "expr": true}, {"t": "8xy − 5x + 8y − 5 = (x + 1)(__B1__)", "a": {"B1": "8y-5"}, "expr": true}], "sol": "8xy + 8y = 8y(x + 1).\n−5x − 5 = −5(x + 1).\n8y(x + 1) − 5(x + 1) = (x + 1)(8y − 5). (Method 1, grouping the first two terms, gives x(8y − 5) + 1(8y − 5) = (8y − 5)(x + 1): the same factors.)"}, {"kind": "mcq", "text": "<b>Example 12</b> · 10xy − 5y + 8 − 16x factorised is", "opts": ["(1 − 2x)(5y − 8)", "(2x − 1)(5y + 8)", "(2x − 1)(5y − 8)", "(2x + 1)(5y − 8)"], "correct": 2, "tag": "", "sol": "10xy − 5y = 5y(2x − 1); write 8 − 16x = −16x + 8 = −8(2x − 1). So 5y(2x − 1) − 8(2x − 1) = (2x − 1)(5y − 8)."}, {"kind": "blank", "p": "<b>Try This (p. 102) · Q1(a)</b> · Factorise 2xy + 6x + y + 3.", "tag": "", "marks": "", "flat": [{"t": "2xy + 6x = 2x(__B1__)", "a": {"B1": "y+3"}, "expr": true}, {"t": "2xy + 6x + y + 3 = (y + 3)(__B1__)", "a": {"B1": "2x+1"}, "expr": true}], "sol": "2xy + 6x = 2x(y + 3) and y + 3 = 1(y + 3).\n2x(y + 3) + 1(y + 3) = (y + 3)(2x + 1)."}, {"kind": "mcq", "text": "<b>Try This (p. 102) · Q1(b)</b> · 6ab − 5a + 6b − 5 factorised is", "opts": ["(6a − 5)(b + 1)", "(6b − 5)(a − 1)", "(6b + 5)(a − 1)", "(6b − 5)(a + 1)"], "correct": 3, "tag": "", "sol": "6ab − 5a = a(6b − 5) and 6b − 5 = 1(6b − 5): a(6b − 5) + 1(6b − 5) = (6b − 5)(a + 1)."}, {"kind": "blank", "p": "<b>Ex 8B · Q1(a–c)</b> · Regroup and factorise.", "tag": "", "marks": "", "flat": [{"t": "a) x<sup>2</sup> + xy + 9x + 9y = x(__B1__) + 9(__B2__)", "a": {"B1": "x+y", "B2": "x+y"}, "expr": true}, {"t": "a) x<sup>2</sup> + xy + 9x + 9y = (x + y)(__B1__)", "a": {"B1": "x+9"}, "expr": true}, {"t": "b) 15xy + 6x + 10y + 4 = 3x(__B1__) + 2(__B2__)", "a": {"B1": "5y+2", "B2": "5y+2"}, "expr": true}, {"t": "b) 15xy + 6x + 10y + 4 = (5y + 2)(__B1__)", "a": {"B1": "3x+2"}, "expr": true}, {"t": "c) 4x<sup>2</sup> − 16xy − 3x + 12y = 4x(__B1__) − 3(__B2__)", "a": {"B1": "x-4y", "B2": "x-4y"}, "expr": true}, {"t": "c) 4x<sup>2</sup> − 16xy − 3x + 12y = (x − 4y)(__B1__)", "a": {"B1": "4x-3"}, "expr": true}], "sol": "a) x<sup>2</sup> + xy = x(x + y), 9x + 9y = 9(x + y).\na) (x + y)(x + 9).\nb) 15xy + 6x = 3x(5y + 2), 10y + 4 = 2(5y + 2).\nb) (5y + 2)(3x + 2).\nc) 4x<sup>2</sup> − 16xy = 4x(x − 4y), −3x + 12y = −3(x − 4y).\nc) (x − 4y)(4x − 3)."}, {"kind": "mcq", "text": "<b>Ex 8B · Q1(d)</b> · 12ab − 8b − 6 + 9a regrouped and factorised is", "opts": ["(3a + 2)(4b − 3)", "(3a − 2)(4b − 3)", "(3a − 2)(4b + 3)", "(4a − 2)(3b + 3)"], "correct": 2, "tag": "", "sol": "12ab − 8b = 4b(3a − 2) and −6 + 9a = 3(3a − 2): 4b(3a − 2) + 3(3a − 2) = (3a − 2)(4b + 3)."}, {"kind": "blank", "p": "<b>Ex 8B · Q1(e, f)</b> · Regroup and factorise.", "tag": "", "marks": "", "flat": [{"t": "e) 3xy − 4y − 3x + 4 = y(__B1__) − 1(__B2__)", "a": {"B1": "3x-4", "B2": "3x-4"}, "expr": true}, {"t": "e) 3xy − 4y − 3x + 4 = (3x − 4)(__B1__)", "a": {"B1": "y-1"}, "expr": true}, {"t": "f) ab + 4a − 3b − 12 = a(__B1__) − 3(__B2__)", "a": {"B1": "b+4", "B2": "b+4"}, "expr": true}, {"t": "f) ab + 4a − 3b − 12 = (b + 4)(__B1__)", "a": {"B1": "a-3"}, "expr": true}], "sol": "e) 3xy − 4y = y(3x − 4), −3x + 4 = −1(3x − 4).\ne) (3x − 4)(y − 1).\nf) ab + 4a = a(b + 4), −3b − 12 = −3(b + 4).\nf) (b + 4)(a − 3)."}, {"kind": "mcq", "text": "<b>Ex 8B · Q1(g)</b> · 9x<sup>2</sup> + 3xy − 12x − 4y regrouped and factorised is", "opts": ["(3x + y)(3x + 4)", "(3x − y)(3x + 4)", "(3x + 4y)(3x − 1)", "(3x + y)(3x − 4)"], "correct": 3, "tag": "", "sol": "9x<sup>2</sup> + 3xy = 3x(3x + y) and −12x − 4y = −4(3x + y): (3x + y)(3x − 4)."}, {"kind": "blank", "p": "<b>Ex 8B · Q1(h)</b> · Regroup and factorise.", "tag": "", "marks": "", "flat": [{"t": "h) 12x − 8 + 3xy − 2y = 4(__B1__) + y(__B2__)", "a": {"B1": "3x-2", "B2": "3x-2"}, "expr": true}, {"t": "h) 12x − 8 + 3xy − 2y = (3x − 2)(__B1__)", "a": {"B1": "y+4"}, "expr": true}], "sol": "h) 12x − 8 = 4(3x − 2), 3xy − 2y = y(3x − 2).\nh) 4(3x − 2) + y(3x − 2) = (3x − 2)(y + 4)."}]}, {"id": "s4", "label": "8.4 Identities", "sub": "Factorisation using (a ± b)² and a² − b², and quick products", "slides": [{"kind": "blank", "p": "<b>Example 13</b> · Factorise x<sup>2</sup> + 4x + 4.", "tag": "", "marks": "", "flat": [{"t": "x<sup>2</sup> + 4x + 4 = (__B1__)<sup>2</sup>", "a": {"B1": "x+2"}, "expr": true, "accept": ["x+2", "-(x+2)"]}], "sol": "x<sup>2</sup> = (x)<sup>2</sup>, 4 = 2<sup>2</sup>, 4x = 2 × x × 2: x<sup>2</sup> + 4x + 4 = (x + 2)<sup>2</sup>."}, {"kind": "mcq", "text": "<b>Example 14</b> · x<sup>2</sup> − 6x + 9 can be written as", "opts": ["(x − 3)(x + 3), the difference of squares", "(x − 3)<sup>2</sup> only, never (3 − x)<sup>2</sup>", "both (x − 3)<sup>2</sup> and (3 − x)<sup>2</sup>", "(x + 3)<sup>2</sup>, since 9 = (+3)<sup>2</sup>"], "correct": 2, "tag": "", "sol": "−6x = 2 × x × (−3) = 2 × (−x) × 3, so x<sup>2</sup> − 6x + 9 = (x − 3)<sup>2</sup> = (3 − x)<sup>2</sup>: a square is the same for a number and its negative."}, {"kind": "blank", "p": "<b>Example 15</b> · Factorise x<sup>2</sup> − 16. Write the bracket with + first.", "tag": "", "marks": "", "flat": [{"t": "x<sup>2</sup> − 16 = (__B1__)(__B2__)", "a": {"B1": "x+4", "B2": "x-4"}, "expr": true}], "sol": "x<sup>2</sup> − 16 = (x)<sup>2</sup> − (4)<sup>2</sup> = (x + 4)(x − 4). Check: x<sup>2</sup> − 4x + 4x − 16 = x<sup>2</sup> − 16."}, {"kind": "mcq", "text": "<b>Example 16</b> · p<sup>2</sup> + 25q<sup>2</sup> + 10pq − 36 factorised is", "opts": ["(p + 5q + 6)(p + 5q − 6)", "(p − 5q + 6)(p − 5q − 6)", "(p + 5q + 6)(p − 5q − 6)", "(p + 5q − 6)<sup>2</sup>"], "correct": 0, "tag": "", "sol": "(p<sup>2</sup> + 10pq + 25q<sup>2</sup>) − 36 = (p + 5q)<sup>2</sup> − 6<sup>2</sup> = (p + 5q + 6)(p + 5q − 6)."}, {"kind": "blank", "p": "<b>Example 17</b> · Factorise 25x<sup>2</sup> − 4y<sup>2</sup> − 9z<sup>2</sup> + 12yz.", "tag": "", "marks": "", "flat": [{"t": "4y<sup>2</sup> − 12yz + 9z<sup>2</sup> = (__B1__)<sup>2</sup>", "a": {"B1": "2y-3z"}, "expr": true, "accept": ["2y-3z", "-(2y-3z)"]}, {"t": "25x<sup>2</sup> − 4y<sup>2</sup> − 9z<sup>2</sup> + 12yz = (5x + 2y − 3z)(__B1__)", "a": {"B1": "5x-2y+3z"}, "expr": true}], "sol": "25x<sup>2</sup> − 4y<sup>2</sup> − 9z<sup>2</sup> + 12yz = 25x<sup>2</sup> − (4y<sup>2</sup> − 12yz + 9z<sup>2</sup>), and 4y<sup>2</sup> − 12yz + 9z<sup>2</sup> = (2y − 3z)<sup>2</sup>.\n(5x)<sup>2</sup> − (2y − 3z)<sup>2</sup> = [5x + (2y − 3z)][5x − (2y − 3z)] = (5x + 2y − 3z)(5x − 2y + 3z)."}, {"kind": "blank", "p": "<b>Example 18</b> · Evaluate 703 × 697.", "tag": "", "marks": "", "flat": [{"t": "703 × 697 = (700 + 3)(700 − 3) = 700<sup>2</sup> − __B1__<sup>2</sup>", "a": {"B1": "3"}}, {"t": "= __B1__", "a": {"B1": "489991"}}], "sol": "(a + b)(a − b) = a<sup>2</sup> − b<sup>2</sup> with a = 700, b = 3.\n490000 − 9 = 489991."}, {"kind": "mcq", "text": "<b>Example 19</b> · Using (a + b)(a − b) = a<sup>2</sup> − b<sup>2</sup>, 40.7 × 39.3 =", "opts": ["1600.51", "1599.49", "1600.49", "1599.51"], "correct": 3, "tag": "", "sol": "(40 + 0.7)(40 − 0.7) = 40<sup>2</sup> − 0.7<sup>2</sup> = 1600 − 0.49 = 1599.51."}, {"kind": "blank", "p": "<b>Example 20</b> · Evaluate (888)<sup>2</sup> − (112)<sup>2</sup>.", "tag": "", "marks": "", "flat": [{"t": "(888 + 112)(888 − 112) = 1000 × __B1__", "a": {"B1": "776"}}, {"t": "= __B1__", "a": {"B1": "776000"}}], "sol": "888 − 112 = 776.\n1000 × 776 = 776000."}, {"kind": "mcq", "text": "<b>Example 21</b> · 64(a + b)<sup>2</sup> − 25(c + d)<sup>2</sup> factorised is", "opts": ["(8a + 8b + 5c + 5d)(8a + 8b − 5c − 5d)", "(8a + b + 5c + d)(8a + b − 5c − d)", "(8a + 8b − 5c + 5d)(8a + 8b + 5c − 5d)", "(8a + 8b + 5c + 5d)<sup>2</sup>"], "correct": 0, "tag": "", "sol": "[8(a + b)]<sup>2</sup> − [5(c + d)]<sup>2</sup> = (8a + 8b)<sup>2</sup> − (5c + 5d)<sup>2</sup> = (8a + 8b + 5c + 5d)(8a + 8b − 5c − 5d)."}, {"kind": "blank", "p": "<b>Example 22</b> · Factorise 11p<sup>5</sup> − 44p. Write the bracket with + first.", "tag": "", "marks": "", "flat": [{"t": "11p<sup>5</sup> − 44p = 11p(__B1__)", "a": {"B1": "p^4-4"}, "expr": true}, {"t": "11p<sup>5</sup> − 44p = 11p(__B1__)(__B2__)", "a": {"B1": "p^2+2", "B2": "p^2-2"}, "expr": true}], "sol": "Take out the HCF 11p first: 11p(p<sup>4</sup> − 4).\np<sup>4</sup> − 4 = (p<sup>2</sup>)<sup>2</sup> − 2<sup>2</sup> = (p<sup>2</sup> + 2)(p<sup>2</sup> − 2), so 11p<sup>5</sup> − 44p = 11p(p<sup>2</sup> + 2)(p<sup>2</sup> − 2)."}, {"kind": "mcq", "text": "<b>Example 23</b> · x<sup>8</sup> − y<sup>8</sup> factorised <b>completely</b> is", "opts": ["(x<sup>4</sup> + y<sup>4</sup>)(x<sup>4</sup> − y<sup>4</sup>)", "(x + y)<sup>4</sup>(x − y)<sup>4</sup>", "(x<sup>2</sup> + y<sup>2</sup>)<sup>2</sup>(x + y)(x − y)", "(x<sup>4</sup> + y<sup>4</sup>)(x<sup>2</sup> + y<sup>2</sup>)(x + y)(x − y)"], "correct": 3, "tag": "", "sol": "(x<sup>4</sup> + y<sup>4</sup>)(x<sup>4</sup> − y<sup>4</sup>) is true but not complete: x<sup>4</sup> − y<sup>4</sup> = (x<sup>2</sup> + y<sup>2</sup>)(x<sup>2</sup> − y<sup>2</sup>) and x<sup>2</sup> − y<sup>2</sup> = (x + y)(x − y). So x<sup>8</sup> − y<sup>8</sup> = (x<sup>4</sup> + y<sup>4</sup>)(x<sup>2</sup> + y<sup>2</sup>)(x + y)(x − y)."}, {"kind": "blank", "p": "<b>Example 24</b> · Factorise p<sup>4</sup>q<sup>12</sup> − p<sup>12</sup>q<sup>4</sup>.", "tag": "", "marks": "", "flat": [{"t": "p<sup>4</sup>q<sup>12</sup> − p<sup>12</sup>q<sup>4</sup> = p<sup>4</sup>q<sup>4</sup>(__B1__)", "a": {"B1": "q^8-p^8"}, "expr": true}, {"t": "p<sup>4</sup>q<sup>12</sup> − p<sup>12</sup>q<sup>4</sup> = p<sup>4</sup>q<sup>4</sup>(q<sup>4</sup> + p<sup>4</sup>)(q<sup>2</sup> + p<sup>2</sup>)(q + p)(__B1__)", "a": {"B1": "q-p"}, "expr": true}], "sol": "HCF = p<sup>4</sup>q<sup>4</sup>: p<sup>4</sup>q<sup>4</sup>(q<sup>8</sup> − p<sup>8</sup>).\nq<sup>8</sup> − p<sup>8</sup> = (q<sup>4</sup> + p<sup>4</sup>)(q<sup>4</sup> − p<sup>4</sup>) = (q<sup>4</sup> + p<sup>4</sup>)(q<sup>2</sup> + p<sup>2</sup>)(q<sup>2</sup> − p<sup>2</sup>) = (q<sup>4</sup> + p<sup>4</sup>)(q<sup>2</sup> + p<sup>2</sup>)(q + p)(q − p)."}, {"kind": "blank", "p": "<b>Example 25</b> · Factorise 112x<sup>2</sup> − 63y<sup>2</sup>. First blank: the HCF; then write the bracket with + first.", "tag": "", "marks": "", "flat": [{"t": "112x<sup>2</sup> − 63y<sup>2</sup> = __B1__(__B2__)(__B3__)", "a": {"B1": "7", "B2": "4x+3y", "B3": "4x-3y"}, "expr": true}], "sol": "112x<sup>2</sup> − 63y<sup>2</sup> = 7(16x<sup>2</sup> − 9y<sup>2</sup>) = 7[(4x)<sup>2</sup> − (3y)<sup>2</sup>] = 7(4x + 3y)(4x − 3y)."}, {"kind": "mcq", "text": "<b>Example 26</b> · p<sup>4</sup> + p<sup>2</sup> + 1 factorised is", "opts": ["(p<sup>2</sup> + p + 1)<sup>2</sup>", "(p<sup>2</sup> + p − 1)(p<sup>2</sup> − p − 1)", "(p<sup>2</sup> + 1)<sup>2</sup>", "(p<sup>2</sup> + p + 1)(p<sup>2</sup> − p + 1)"], "correct": 3, "tag": "", "sol": "Add and subtract p<sup>2</sup>: p<sup>4</sup> + 2p<sup>2</sup> + 1 − p<sup>2</sup> = (p<sup>2</sup> + 1)<sup>2</sup> − p<sup>2</sup> = (p<sup>2</sup> + 1 + p)(p<sup>2</sup> + 1 − p) = (p<sup>2</sup> + p + 1)(p<sup>2</sup> − p + 1)."}, {"kind": "blank", "p": "<b>Try This (p. 102) · Q1(c–e)</b> · Factorise. In (c), write the bracket with + first.", "tag": "", "marks": "", "flat": [{"t": "c) x<sup>2</sup> − 144 = (__B1__)(__B2__)", "a": {"B1": "x+12", "B2": "x-12"}, "expr": true}, {"t": "d) x<sup>2</sup> + 6x + 9 = (__B1__)<sup>2</sup>", "a": {"B1": "x+3"}, "expr": true, "accept": ["x+3", "-(x+3)"]}, {"t": "e) x<sup>2</sup> − 10x + 25 = (__B1__)<sup>2</sup>", "a": {"B1": "x-5"}, "expr": true, "accept": ["x-5", "-(x-5)"]}], "sol": "c) x<sup>2</sup> − 12<sup>2</sup> = (x + 12)(x − 12).\nd) 6x = 2 × x × 3: (x + 3)<sup>2</sup>.\ne) −10x = 2 × x × (−5): (x − 5)<sup>2</sup> or (5 − x)<sup>2</sup>."}, {"kind": "mcq", "text": "<b>Try This (p. 102) · Q2</b> · 89 × 111 =", "opts": ["9889", "10121", "9879", "9779"], "correct": 2, "tag": "", "sol": "(100 − 11)(100 + 11) = 100<sup>2</sup> − 11<sup>2</sup> = 10000 − 121 = 9879."}, {"kind": "blank", "p": "<b>Ex 8B · Q2(a–e)</b> · Factorise the following using appropriate identity.", "tag": "", "marks": "", "flat": [{"t": "a) a<sup>2</sup> + 6a + 9 = (__B1__)<sup>2</sup>", "a": {"B1": "a+3"}, "expr": true, "accept": ["a+3", "-(a+3)"]}, {"t": "b) 4x<sup>2</sup> + 4xy + y<sup>2</sup> = (__B1__)<sup>2</sup>", "a": {"B1": "2x+y"}, "expr": true, "accept": ["2x+y", "-(2x+y)"]}, {"t": "c) 4a<sup>2</sup> + 20ab + 25b<sup>2</sup> = (__B1__)<sup>2</sup>", "a": {"B1": "2a+5b"}, "expr": true, "accept": ["2a+5b", "-(2a+5b)"]}, {"t": "d) 25x<sup>2</sup> − 20xy + 4y<sup>2</sup> = (__B1__)<sup>2</sup>", "a": {"B1": "5x-2y"}, "expr": true, "accept": ["5x-2y", "-(5x-2y)"]}, {"t": "e) 4x<sup>2</sup> − 12x + 9 = (__B1__)<sup>2</sup>", "a": {"B1": "2x-3"}, "expr": true, "accept": ["2x-3", "-(2x-3)"]}], "sol": "a) 6a = 2 × a × 3: (a + 3)<sup>2</sup>.\nb) (2x)<sup>2</sup> + 2(2x)(y) + y<sup>2</sup> = (2x + y)<sup>2</sup>.\nc) (2a)<sup>2</sup> + 2(2a)(5b) + (5b)<sup>2</sup> = (2a + 5b)<sup>2</sup>.\nd) (5x)<sup>2</sup> − 2(5x)(2y) + (2y)<sup>2</sup> = (5x − 2y)<sup>2</sup>.\ne) (2x)<sup>2</sup> − 2(2x)(3) + 3<sup>2</sup> = (2x − 3)<sup>2</sup>."}, {"kind": "mcq", "text": "<b>Ex 8B · Q2(f)</b> · 121 + 22x + x<sup>2</sup> factorised is", "opts": ["(11 + x)<sup>2</sup>", "(11 − x)<sup>2</sup>", "(x + 22)<sup>2</sup>", "(11 + x)(11 − x)"], "correct": 0, "tag": "", "sol": "121 = 11<sup>2</sup> and 22x = 2 × 11 × x: (11 + x)<sup>2</sup>."}, {"kind": "blank", "p": "<b>Ex 8B · Q2(g–j)</b> · Factorise the following using appropriate identity. For a difference of squares, write the bracket with + first.", "tag": "", "marks": "", "flat": [{"t": "g) x<sup>2</sup> − y<sup>2</sup> = (__B1__)(__B2__)", "a": {"B1": "x+y", "B2": "x-y"}, "expr": true}, {"t": "h) 16a<sup>2</sup> + 88a + 121 = (__B1__)<sup>2</sup>", "a": {"B1": "4a+11"}, "expr": true, "accept": ["4a+11", "-(4a+11)"]}, {"t": "i) 4p<sup>2</sup> − 16q<sup>2</sup> = (__B1__)(__B2__)", "a": {"B1": "2p+4q", "B2": "2p-4q"}, "expr": true}, {"t": "j) 25x<sup>2</sup> − 49y<sup>2</sup> = (__B1__)(__B2__)", "a": {"B1": "5x+7y", "B2": "5x-7y"}, "expr": true}], "sol": "g) (x + y)(x − y).\nh) (4a)<sup>2</sup> + 2(4a)(11) + 11<sup>2</sup> = (4a + 11)<sup>2</sup>.\ni) (2p)<sup>2</sup> − (4q)<sup>2</sup> = (2p + 4q)(2p − 4q), which can be tidied to 4(p + 2q)(p − 2q).\nj) (5x)<sup>2</sup> − (7y)<sup>2</sup> = (5x + 7y)(5x − 7y)."}, {"kind": "mcq", "text": "<b>Ex 8B · Q2(k)</b> · 36x<sup>2</sup> − 84xy + 49y<sup>2</sup> factorised is", "opts": ["(6x − 7y)(6x + 7y)", "(6x − 14y)<sup>2</sup>", "(6x − 7y)<sup>2</sup>", "(6x + 7y)<sup>2</sup>"], "correct": 2, "tag": "", "sol": "(6x)<sup>2</sup> − 2(6x)(7y) + (7y)<sup>2</sup> = (6x − 7y)<sup>2</sup>."}, {"kind": "blank", "p": "<b>Ex 8B · Q2(l)</b> · Factorise completely.", "tag": "", "marks": "", "flat": [{"t": "x<sup>4</sup> − 81 = (x<sup>2</sup> + __B1__)(x + __B2__)(x − __B3__)", "a": {"B1": "9", "B2": "3", "B3": "3"}, "expr": true}], "sol": "x<sup>4</sup> − 81 = (x<sup>2</sup>)<sup>2</sup> − 9<sup>2</sup> = (x<sup>2</sup> + 9)(x<sup>2</sup> − 9) = (x<sup>2</sup> + 9)(x + 3)(x − 3)."}, {"kind": "mcq", "text": "<b>Ex 8B · Q2(m)</b> · m<sup>6</sup> − 25 factorised is", "opts": ["(m<sup>3</sup> + 25)(m<sup>3</sup> − 1)", "(m<sup>3</sup> + 5)(m<sup>3</sup> − 5)", "(m<sup>3</sup> − 5)<sup>2</sup>", "(m<sup>2</sup> + 5)(m<sup>2</sup> − 5)"], "correct": 1, "tag": "", "sol": "m<sup>6</sup> = (m<sup>3</sup>)<sup>2</sup> and 25 = 5<sup>2</sup>: (m<sup>3</sup> + 5)(m<sup>3</sup> − 5)."}, {"kind": "blank", "p": "<b>Ex 8B · Q2(n–r)</b> · Factorise using appropriate identity. For a difference of squares, write the bracket with + first.", "tag": "", "marks": "", "flat": [{"t": "n) 64x<sup>2</sup> + 16x + 1 = (__B1__)<sup>2</sup>", "a": {"B1": "8x+1"}, "expr": true, "accept": ["8x+1", "-(8x+1)"]}, {"t": "o) 144 − 24x + x<sup>2</sup> = (__B1__)<sup>2</sup>", "a": {"B1": "12-x"}, "expr": true, "accept": ["12-x", "-(12-x)"]}, {"t": "p) 25a<sup>2</sup>b<sup>2</sup> − 9p<sup>2</sup>q<sup>2</sup> = (__B1__)(__B2__)", "a": {"B1": "5ab+3pq", "B2": "5ab-3pq"}, "expr": true}, {"t": "q) 36a<sup>2</sup> − 36ab + 9b<sup>2</sup> = (__B1__)<sup>2</sup>", "a": {"B1": "6a-3b"}, "expr": true, "accept": ["6a-3b", "-(6a-3b)"]}, {"t": "r) 64 − 9t<sup>2</sup> = (__B1__)(__B2__)", "a": {"B1": "8+3t", "B2": "8-3t"}, "expr": true}], "sol": "n) (8x)<sup>2</sup> + 2(8x)(1) + 1 = (8x + 1)<sup>2</sup>.\no) 12<sup>2</sup> − 2(12)(x) + x<sup>2</sup> = (12 − x)<sup>2</sup> = (x − 12)<sup>2</sup>.\np) (5ab)<sup>2</sup> − (3pq)<sup>2</sup> = (5ab + 3pq)(5ab − 3pq).\nq) (6a)<sup>2</sup> − 2(6a)(3b) + (3b)<sup>2</sup> = (6a − 3b)<sup>2</sup> (= 9(2a − b)<sup>2</sup>).\nr) 8<sup>2</sup> − (3t)<sup>2</sup> = (8 + 3t)(8 − 3t)."}, {"kind": "mcq", "text": "<b>Ex 8B · Q2(s)</b> · 9a<sup>2</sup>x<sup>2</sup> − 12axb + 4b<sup>2</sup> factorised is", "opts": ["(3ax − 2b)(3ax + 2b)", "(9ax − 4b)<sup>2</sup>", "(3ax + 2b)<sup>2</sup>", "(3ax − 2b)<sup>2</sup>"], "correct": 3, "tag": "", "sol": "(3ax)<sup>2</sup> − 2(3ax)(2b) + (2b)<sup>2</sup> = (3ax − 2b)<sup>2</sup>."}, {"kind": "blank", "p": "<b>Ex 8B · Q2(t)</b> · Factorise using appropriate identity. Write the bracket with + first.", "tag": "", "marks": "", "flat": [{"t": "d<sup>2</sup>/100 − e<sup>2</sup>/9 = (__B1__)(__B2__)", "a": {"B1": "d/10+e/3", "B2": "d/10-e/3"}, "expr": true}], "sol": "d<sup>2</sup>/100 = ({d/10})<sup>2</sup> and e<sup>2</sup>/9 = ({e/3})<sup>2</sup>: ({d/10} + {e/3})({d/10} − {e/3}). Type e.g. d/10+e/3."}, {"kind": "blank", "p": "<b>Ex 8B · Q3(a)</b> · Factorise 16p<sup>2</sup> − 8pq − 25r<sup>2</sup> + q<sup>2</sup>.", "tag": "", "marks": "", "flat": [{"t": "16p<sup>2</sup> − 8pq + q<sup>2</sup> = (__B1__)<sup>2</sup>", "a": {"B1": "4p-q"}, "expr": true, "accept": ["4p-q", "-(4p-q)"]}, {"t": "16p<sup>2</sup> − 8pq − 25r<sup>2</sup> + q<sup>2</sup> = (4p − q + 5r)(__B1__)", "a": {"B1": "4p-q-5r"}, "expr": true}], "sol": "Group the square: 16p<sup>2</sup> − 8pq + q<sup>2</sup> = (4p − q)<sup>2</sup>.\n(4p − q)<sup>2</sup> − (5r)<sup>2</sup> = (4p − q + 5r)(4p − q − 5r)."}, {"kind": "mcq", "text": "<b>Ex 8B · Q3(b)</b> · x<sup>2</sup> + 2xy + y<sup>2</sup> − 1 factorised is", "opts": ["(x + y + 1)(x + y − 1)", "(x + y + 1)<sup>2</sup>", "(x − y + 1)(x − y − 1)", "(x + y − 1)<sup>2</sup>"], "correct": 0, "tag": "", "sol": "(x + y)<sup>2</sup> − 1<sup>2</sup> = (x + y + 1)(x + y − 1)."}, {"kind": "blank", "p": "<b>Ex 8B · Q3(c)</b> · Factorise 64x<sup>2</sup>y<sup>2</sup> − 9x<sup>2</sup> + 18xy − 9y<sup>2</sup>.", "tag": "", "marks": "", "flat": [{"t": "9x<sup>2</sup> − 18xy + 9y<sup>2</sup> = (__B1__)<sup>2</sup>", "a": {"B1": "3x-3y"}, "expr": true, "accept": ["3x-3y", "-(3x-3y)"]}, {"t": "64x<sup>2</sup>y<sup>2</sup> − 9x<sup>2</sup> + 18xy − 9y<sup>2</sup> = (8xy + 3x − 3y)(__B1__)", "a": {"B1": "8xy-3x+3y"}, "expr": true}], "sol": "−9x<sup>2</sup> + 18xy − 9y<sup>2</sup> = −(9x<sup>2</sup> − 18xy + 9y<sup>2</sup>) = −(3x − 3y)<sup>2</sup>.\n(8xy)<sup>2</sup> − (3x − 3y)<sup>2</sup> = (8xy + 3x − 3y)(8xy − 3x + 3y)."}, {"kind": "mcq", "text": "<b>Ex 8B · Q3(d)</b> · 9(x − y)<sup>2</sup> − 16(p − 2q)<sup>2</sup> factorised is", "opts": ["(3x − y + 4p − 2q)(3x − y − 4p + 2q)", "(9x − 9y + 16p − 32q)(9x − 9y − 16p + 32q)", "(3x − 3y + 4p − 8q)<sup>2</sup>", "(3x − 3y + 4p − 8q)(3x − 3y − 4p + 8q)"], "correct": 3, "tag": "", "sol": "[3(x − y)]<sup>2</sup> − [4(p − 2q)]<sup>2</sup> = (3x − 3y + 4p − 8q)(3x − 3y − 4p + 8q)."}, {"kind": "blank", "p": "<b>Ex 8B · Q3(e)</b> · Factorise (6a − 5b)<sup>2</sup> − 9(a − 2b)<sup>2</sup> = [(6a − 5b) + 3(a − 2b)][(6a − 5b) − 3(a − 2b)]. Simplify each bracket.", "tag": "", "marks": "", "flat": [{"t": "(6a − 5b) + 3(a − 2b) = __B1__", "a": {"B1": "9a-11b"}, "expr": true}, {"t": "(6a − 5b) − 3(a − 2b) = __B1__", "a": {"B1": "3a+b"}, "expr": true}], "sol": "9(a − 2b)<sup>2</sup> = [3(a − 2b)]<sup>2</sup>. 6a − 5b + 3a − 6b = 9a − 11b.\n6a − 5b − 3a + 6b = 3a + b. So the answer is (9a − 11b)(3a + b)."}, {"kind": "blank", "p": "<b>Ex 8B · Q3(f)</b> · Factorise (a − 5)<sup>2</sup> − (a − 8)<sup>2</sup>. First blank: the number in front.", "tag": "", "marks": "", "flat": [{"t": "(a − 5)<sup>2</sup> − (a − 8)<sup>2</sup> = __B1__(__B2__)", "a": {"B1": "3", "B2": "2a-13"}, "expr": true}], "sol": "[(a − 5) + (a − 8)][(a − 5) − (a − 8)] = (2a − 13)(3) = 3(2a − 13)."}, {"kind": "mcq", "text": "<b>Ex 8B · Q3(g)</b> · 64a<sup>8</sup>b<sup>8</sup> − 36a<sup>2</sup>b<sup>2</sup> factorised completely is", "opts": ["4a<sup>2</sup>b<sup>2</sup>(4a<sup>3</sup>b<sup>3</sup> − 3)<sup>2</sup>", "4a<sup>2</sup>b<sup>2</sup>(4a<sup>4</sup>b<sup>4</sup> + 3)(4a<sup>4</sup>b<sup>4</sup> − 3)", "4a<sup>2</sup>b<sup>2</sup>(4a<sup>3</sup>b<sup>3</sup> + 3)(4a<sup>3</sup>b<sup>3</sup> − 3)", "2a<sup>2</sup>b<sup>2</sup>(4a<sup>3</sup>b<sup>3</sup> + 3)(4a<sup>3</sup>b<sup>3</sup> − 3)"], "correct": 2, "tag": "", "sol": "HCF = 4a<sup>2</sup>b<sup>2</sup>: 4a<sup>2</sup>b<sup>2</sup>(16a<sup>6</sup>b<sup>6</sup> − 9) = 4a<sup>2</sup>b<sup>2</sup>(4a<sup>3</sup>b<sup>3</sup> + 3)(4a<sup>3</sup>b<sup>3</sup> − 3)."}, {"kind": "blank", "p": "<b>Ex 8B · Q3(h)</b> · Factorise 16x<sup>2</sup> − 24xy + 9y<sup>2</sup> − 4x + 3y.", "tag": "", "marks": "", "flat": [{"t": "16x<sup>2</sup> − 24xy + 9y<sup>2</sup> = (__B1__)<sup>2</sup>", "a": {"B1": "4x-3y"}, "expr": true, "accept": ["4x-3y", "-(4x-3y)"]}, {"t": "−4x + 3y = −1(__B1__)", "a": {"B1": "4x-3y"}, "expr": true}, {"t": "16x<sup>2</sup> − 24xy + 9y<sup>2</sup> − 4x + 3y = (4x − 3y)(__B1__)", "a": {"B1": "4x-3y-1"}, "expr": true}], "sol": "16x<sup>2</sup> − 24xy + 9y<sup>2</sup> = (4x − 3y)<sup>2</sup>.\n−4x + 3y = −1(4x − 3y).\n(4x − 3y)<sup>2</sup> − 1(4x − 3y) = (4x − 3y)(4x − 3y − 1)."}, {"kind": "mcq", "text": "<b>Ex 8B · Q3(i)</b> · m<sup>4</sup> + m<sup>2</sup> + 1 factorised is", "opts": ["(m<sup>2</sup> + 1)<sup>2</sup>", "(m<sup>2</sup> + m − 1)(m<sup>2</sup> − m − 1)", "(m<sup>2</sup> + m + 1)(m<sup>2</sup> − m + 1)", "(m<sup>2</sup> + m + 1)<sup>2</sup>"], "correct": 2, "tag": "", "sol": "Add and subtract m<sup>2</sup>: (m<sup>2</sup> + 1)<sup>2</sup> − m<sup>2</sup> = (m<sup>2</sup> + m + 1)(m<sup>2</sup> − m + 1)."}, {"kind": "blank", "p": "<b>Ex 8B · Q4(a–c)</b> · Evaluate using identities.", "tag": "", "marks": "", "flat": [{"t": "a) 307 × 293 = 300<sup>2</sup> − 7<sup>2</sup> = __B1__", "a": {"B1": "89951"}}, {"t": "b) 4.15 × 3.85 = 4<sup>2</sup> − 0.15<sup>2</sup> = __B1__", "a": {"B1": "15.9775"}}, {"t": "c) (72)<sup>2</sup> − (68)<sup>2</sup> = 140 × 4 = __B1__", "a": {"B1": "560"}}], "sol": "a) (300 + 7)(300 − 7) = 90000 − 49 = 89951.\nb) (4 + 0.15)(4 − 0.15) = 16 − 0.0225 = 15.9775.\nc) (72 + 68)(72 − 68) = 140 × 4 = 560."}, {"kind": "mcq", "text": "<b>Ex 8B · Q4(d)</b> · (7.5)<sup>2</sup> − (2.5)<sup>2</sup> =", "opts": ["50", "25", "60", "5"], "correct": 0, "tag": "", "sol": "(7.5 + 2.5)(7.5 − 2.5) = 10 × 5 = 50."}, {"kind": "blank", "p": "<b>Ex 8B · Q4(e)</b> · Evaluate using an identity.", "tag": "", "marks": "", "flat": [{"t": "67 × 73 = (70 − 3)(70 + 3) = __B1__", "a": {"B1": "4891"}}], "sol": "70<sup>2</sup> − 3<sup>2</sup> = 4900 − 9 = 4891."}]}, {"id": "s5", "label": "8.5 x² + ax + b", "sub": "Factorising trinomials x² + ax + b by splitting the middle term (sum and product)", "slides": [{"kind": "blank", "p": "<b>Example 27</b> · Factorise x<sup>2</sup> + 20x + 91. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "Two numbers with product 91 and sum 20: __B1__", "a": {"B1": "7, 13"}, "expr": "list", "accept": ["13, 7"]}, {"t": "x<sup>2</sup> + 20x + 91 = (x + 7)(__B1__)", "a": {"B1": "x+13"}, "expr": true}], "sol": "7 × 13 = 91 and 7 + 13 = 20.\nProduct = 91, sum = 20: the numbers are 7 and 13. x<sup>2</sup> + 20x + 91 = x<sup>2</sup> + 7x + 13x + 91 = x(x + 7) + 13(x + 7) = (x + 7)(x + 13)."}, {"kind": "mcq", "text": "<b>Example 28</b> · x<sup>2</sup> + 7x + 6 factorised is", "opts": ["(x + 2)(x + 3)", "(x − 1)(x − 6)", "(x + 1)(x + 6)", "(x + 1)(x − 6)"], "correct": 2, "tag": "", "sol": "Product 6, sum 7: 1 and 6. x<sup>2</sup> + x + 6x + 6 = x(x + 1) + 6(x + 1) = (x + 1)(x + 6)."}, {"kind": "blank", "p": "<b>Example 29</b> · Factorise x<sup>2</sup> + 7x − 30. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "Two numbers with product −30 and sum 7: __B1__", "a": {"B1": "-3, 10"}, "expr": "list", "accept": ["10, -3"]}, {"t": "x<sup>2</sup> + 7x − 30 = (x − 3)(__B1__)", "a": {"B1": "x+10"}, "expr": true}], "sol": "−3 × 10 = −30 and −3 + 10 = 7.\nProduct = −30, sum = 7: the numbers are −3 and 10. x<sup>2</sup> + 7x − 30 = x<sup>2</sup> − 3x + 10x − 30 = x(x − 3) + 10(x − 3) = (x − 3)(x + 10)."}, {"kind": "mcq", "text": "<b>Example 30</b> · x<sup>2</sup> + x − 56 factorised is", "opts": ["(x + 4)(x − 14)", "(x − 7)(x + 8)", "(x − 7)(x − 8)", "(x + 7)(x − 8)"], "correct": 1, "tag": "", "sol": "Product −56, sum +1: 8 and −7. x<sup>2</sup> − 7x + 8x − 56 = x(x − 7) + 8(x − 7) = (x − 7)(x + 8)."}, {"kind": "blank", "p": "<b>Example 31</b> · Factorise x<sup>2</sup> − 6x − 40. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "Two numbers with product −40 and sum −6: __B1__", "a": {"B1": "4, -10"}, "expr": "list", "accept": ["-10, 4"]}, {"t": "x<sup>2</sup> − 6x − 40 = (x + 4)(__B1__)", "a": {"B1": "x-10"}, "expr": true}], "sol": "4 × −10 = −40 and 4 + −10 = −6.\nProduct = −40, sum = −6: the numbers are 4 and −10. x<sup>2</sup> − 6x − 40 = x<sup>2</sup> + 4x − 10x − 40 = x(x + 4) − 10(x + 4) = (x + 4)(x − 10)."}, {"kind": "mcq", "text": "<b>Example 32</b> · x<sup>2</sup> − 7x + 12 factorised is", "opts": ["(x − 3)(x − 4)", "(x − 2)(x − 6)", "(x + 3)(x + 4)", "(x + 3)(x − 4)"], "correct": 0, "tag": "", "sol": "Product +12, sum −7: both numbers negative, −3 and −4. x(x − 3) − 4(x − 3) = (x − 3)(x − 4)."}, {"kind": "blank", "p": "<b>Example 33</b> · Factorise x<sup>2</sup> + 17x + 60. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "Two numbers with product 60 and sum 17: __B1__", "a": {"B1": "5, 12"}, "expr": "list", "accept": ["12, 5"]}, {"t": "x<sup>2</sup> + 17x + 60 = (x + 5)(__B1__)", "a": {"B1": "x+12"}, "expr": true}], "sol": "5 × 12 = 60 and 5 + 12 = 17.\nProduct = 60, sum = 17: the numbers are 5 and 12. x<sup>2</sup> + 17x + 60 = x<sup>2</sup> + 5x + 12x + 60 = x(x + 5) + 12(x + 5) = (x + 5)(x + 12)."}, {"kind": "mcq", "text": "<b>Example 34</b> · x<sup>2</sup> + 5xy − 24y<sup>2</sup> factorised is", "opts": ["(x + 6y)(x − 4y)", "(x + 8y)(x − 3y)", "(x + 8y)(x + 3y)", "(x − 8y)(x + 3y)"], "correct": 1, "tag": "", "sol": "Product −24, sum 5: 8 and −3. x<sup>2</sup> + 8xy − 3xy − 24y<sup>2</sup> = x(x + 8y) − 3y(x + 8y) = (x + 8y)(x − 3y)."}, {"kind": "blank", "p": "<b>Example 35</b> · Factorise (y − x)<sup>2</sup> − 10(x − y) + 25. Let y − x = a.", "tag": "", "marks": "", "flat": [{"t": "Then x − y = __B1__", "a": {"B1": "-a"}, "expr": true}, {"t": "(y − x)<sup>2</sup> − 10(x − y) + 25 = (__B1__)<sup>2</sup>", "a": {"B1": "y-x+5"}, "expr": true, "accept": ["y-x+5", "-(y-x+5)"]}], "sol": "x − y = −(y − x) = −a.\na<sup>2</sup> − 10(−a) + 25 = a<sup>2</sup> + 10a + 25 = (a + 5)<sup>2</sup> = (y − x + 5)<sup>2</sup>."}, {"kind": "blank", "p": "<b>Example 36</b> · If the area of a square is (x<sup>2</sup> + 6x + 9) square units, find its side.", "tag": "", "marks": "", "flat": [{"t": "Side = __B1__ units", "a": {"B1": "x+3"}, "expr": true}], "sol": "x<sup>2</sup> + 6x + 9 = x<sup>2</sup> + 3x + 3x + 9 = x(x + 3) + 3(x + 3) = (x + 3)(x + 3), so the side is (x + 3) units."}, {"kind": "mcq", "text": "<b>Try This (p. 104) · (a)</b> · x<sup>2</sup> + 15x + 56 factorised is", "opts": ["(x + 4)(x + 14)", "(x − 7)(x − 8)", "(x + 7)(x + 8)", "(x + 7)(x − 8)"], "correct": 2, "tag": "", "sol": "Product 56, sum 15: 7 and 8. (x + 7)(x + 8)."}, {"kind": "blank", "p": "<b>Try This (p. 104) · (b, c)</b> · Factorise.", "tag": "", "marks": "", "flat": [{"t": "b) (2a + 5b)<sup>2</sup> − 7(2a + 5b) + 12 = (2a + 5b + __B1__)(2a + 5b + __B2__)   (smaller number first)", "a": {"B1": "-4", "B2": "-3"}}, {"t": "c) Two numbers with product −16 and sum 6: __B1__", "a": {"B1": "-2, 8"}, "expr": "list", "accept": ["8, -2"]}, {"t": "c) x<sup>2</sup> + 6xy − 16y<sup>2</sup> = (x − 2y)(__B1__)", "a": {"B1": "x+8y"}, "expr": true}], "sol": "b) Let 2a + 5b = t: t<sup>2</sup> − 7t + 12 = (t − 4)(t − 3) = (2a + 5b − 4)(2a + 5b − 3).\nc) −2 × 8 = −16 and −2 + 8 = 6.\nc) Product = −16, sum = 6: the numbers are −2 and 8. x<sup>2</sup> + 6xy − 16y<sup>2</sup> = x<sup>2</sup> − 2xy + 8xy − 16y<sup>2</sup> = x(x − 2y) + 8y(x − 2y) = (x − 2y)(x + 8y)."}, {"kind": "blank", "p": "<b>Ex 8C · Q1(a–e)</b> · Find the sum and the product of the following numbers.", "tag": "", "marks": "", "flat": [{"t": "a) 3, 5: sum = __B1__, product = __B2__", "a": {"B1": "8", "B2": "15"}}, {"t": "b) 8, 9: sum = __B1__, product = __B2__", "a": {"B1": "17", "B2": "72"}}, {"t": "c) 9, 5: sum = __B1__, product = __B2__", "a": {"B1": "14", "B2": "45"}}, {"t": "d) 13, 3: sum = __B1__, product = __B2__", "a": {"B1": "16", "B2": "39"}}, {"t": "e) −3, 5: sum = __B1__, product = __B2__", "a": {"B1": "2", "B2": "-15"}}], "sol": "a) 3 + 5 = 8;  3 × 5 = 15.\nb) 8 + 9 = 17;  8 × 9 = 72.\nc) 9 + 5 = 14;  9 × 5 = 45.\nd) 13 + 3 = 16;  13 × 3 = 39.\ne) −3 + 5 = 2;  −3 × 5 = −15."}, {"kind": "mcq", "text": "Which pair of numbers has sum −2 and product −24?", "opts": ["−6 and −4", "−6 and 4", "6 and −4", "−24 and 1"], "correct": 1, "tag": "", "sol": "−6 + 4 = −2 and −6 × 4 = −24. (6 and −4 have sum +2; −6 and −4 have product +24; −24 + 1 = −23.)"}, {"kind": "blank", "p": "<b>Ex 8C · Q1(f–j)</b> · Find the sum and the product of the following numbers.", "tag": "", "marks": "", "flat": [{"t": "f) −7, 3: sum = __B1__, product = __B2__", "a": {"B1": "-4", "B2": "-21"}}, {"t": "g) 8, −3: sum = __B1__, product = __B2__", "a": {"B1": "5", "B2": "-24"}}, {"t": "h) 11, −7: sum = __B1__, product = __B2__", "a": {"B1": "4", "B2": "-77"}}, {"t": "i) −9, −7: sum = __B1__, product = __B2__", "a": {"B1": "-16", "B2": "63"}}, {"t": "j) −12, −2: sum = __B1__, product = __B2__", "a": {"B1": "-14", "B2": "24"}}], "sol": "f) −7 + 3 = −4;  −7 × 3 = −21.\ng) 8 + −3 = 5;  8 × −3 = −24.\nh) 11 + −7 = 4;  11 × −7 = −77.\ni) −9 + −7 = −16;  −9 × −7 = 63.\nj) −12 + −2 = −14;  −12 × −2 = 24."}, {"kind": "blank", "p": "<b>Ex 8C · Q2(b–e)</b> · Given are the sum and product of two numbers. Find the two numbers (type both, separated by a comma). Example: sum 17, product 72 → 8, 9.", "tag": "", "marks": "", "flat": [{"t": "b) sum 8, product 15: numbers = __B1__", "a": {"B1": "3, 5"}, "expr": "list", "accept": ["5, 3"]}, {"t": "c) sum 11, product 30: numbers = __B1__", "a": {"B1": "5, 6"}, "expr": "list", "accept": ["6, 5"]}, {"t": "d) sum −15, product 56: numbers = __B1__", "a": {"B1": "-8, -7"}, "expr": "list", "accept": ["-7, -8"]}, {"t": "e) sum −9, product 18: numbers = __B1__", "a": {"B1": "-6, -3"}, "expr": "list", "accept": ["-3, -6"]}], "sol": "b) 3 + 5 = 8 and 3 × 5 = 15: the numbers are 3 and 5.\nc) 5 + 6 = 11 and 5 × 6 = 30: the numbers are 5 and 6.\nd) −8 + −7 = −15 and −8 × −7 = 56: the numbers are −8 and −7.\ne) −6 + −3 = −9 and −6 × −3 = 18: the numbers are −6 and −3."}, {"kind": "mcq", "text": "Two numbers have a <b>positive</b> product and a <b>negative</b> sum. The numbers are", "opts": ["both positive", "one positive, one negative", "both negative", "one of them is zero"], "correct": 2, "tag": "", "sol": "Positive product: the signs are the same. Negative sum: they cannot both be positive, so both are negative."}, {"kind": "blank", "p": "<b>Ex 8C · Q2(f–h, j)</b> · Given are the sum and product of two numbers. Find the two numbers (type both, separated by a comma). Example: sum −4, product −21 → −7, 3.", "tag": "", "marks": "", "flat": [{"t": "f) sum −9, product 14: numbers = __B1__", "a": {"B1": "-7, -2"}, "expr": "list", "accept": ["-2, -7"]}, {"t": "g) sum 2, product −15: numbers = __B1__", "a": {"B1": "-3, 5"}, "expr": "list", "accept": ["5, -3"]}, {"t": "h) sum 3, product −40: numbers = __B1__", "a": {"B1": "-5, 8"}, "expr": "list", "accept": ["8, -5"]}, {"t": "j) sum −7, product −30: numbers = __B1__", "a": {"B1": "-10, 3"}, "expr": "list", "accept": ["3, -10"]}], "sol": "f) −7 + −2 = −9 and −7 × −2 = 14: the numbers are −7 and −2.\ng) −3 + 5 = 2 and −3 × 5 = −15: the numbers are −3 and 5.\nh) −5 + 8 = 3 and −5 × 8 = −40: the numbers are −5 and 8.\nj) −10 + 3 = −7 and −10 × 3 = −30: the numbers are −10 and 3."}, {"kind": "blank", "p": "<b>Ex 8C · Q3(b–d)</b> · Fill in the blanks (example: x<sup>2</sup> − 5x + 6 → middle term −5, product 6, numbers −2, −3, factors (x − 3)(x − 2)).", "tag": "", "marks": "", "flat": [{"t": "b) y<sup>2</sup> − 11y + 24: sum (middle term) = __B1__, product = __B2__", "a": {"B1": "-11", "B2": "24"}}, {"t": "b) numbers (both, with a comma) = __B1__", "a": {"B1": "-3, -8"}, "expr": "list", "accept": ["-8, -3"]}, {"t": "b) factors: y<sup>2</sup> − 11y + 24 = (y − 3)(__B1__)", "a": {"B1": "y-8"}, "expr": true}, {"t": "c) x<sup>2</sup> − 9x + 14: sum (middle term) = __B1__, product = __B2__", "a": {"B1": "-9", "B2": "14"}}, {"t": "c) numbers (both, with a comma) = __B1__", "a": {"B1": "-2, -7"}, "expr": "list", "accept": ["-7, -2"]}, {"t": "c) factors: x<sup>2</sup> − 9x + 14 = (x − 2)(__B1__)", "a": {"B1": "x-7"}, "expr": true}, {"t": "d) x<sup>2</sup> + x − 6: sum (middle term) = __B1__, product = __B2__", "a": {"B1": "1", "B2": "-6"}}, {"t": "d) numbers (both, with a comma) = __B1__", "a": {"B1": "-2, 3"}, "expr": "list", "accept": ["3, -2"]}, {"t": "d) factors: x<sup>2</sup> + x − 6 = (x − 2)(__B1__)", "a": {"B1": "x+3"}, "expr": true}], "sol": "b) The middle-term coefficient is −11 and the product is 24.\nb) −3 × −8 = 24 and −3 + −8 = −11.\nb) Product = 24, sum = −11: the numbers are −3 and −8. y<sup>2</sup> − 11y + 24 = y<sup>2</sup> − 3y − 8y + 24 = y(y − 3) − 8(y − 3) = (y − 3)(y − 8).\nc) The middle-term coefficient is −9 and the product is 14.\nc) −2 × −7 = 14 and −2 + −7 = −9.\nc) Product = 14, sum = −9: the numbers are −2 and −7. x<sup>2</sup> − 9x + 14 = x<sup>2</sup> − 2x − 7x + 14 = x(x − 2) − 7(x − 2) = (x − 2)(x − 7).\nd) The middle-term coefficient is 1 and the product is −6.\nd) −2 × 3 = −6 and −2 + 3 = 1.\nd) Product = −6, sum = 1: the numbers are −2 and 3. x<sup>2</sup> + x − 6 = x<sup>2</sup> − 2x + 3x − 6 = x(x − 2) + 3(x − 2) = (x − 2)(x + 3)."}, {"kind": "mcq", "text": "For x<sup>2</sup> − 3x − 70, the two numbers must have", "opts": ["sum 3 and product 70", "sum −70 and product −3", "sum −3 and product −70", "sum 70 and product −3"], "correct": 2, "tag": "", "sol": "x<sup>2</sup> + ax + b needs two numbers with sum a (the middle coefficient) and product b (the constant): sum −3, product −70 (the numbers are −10 and 7)."}, {"kind": "blank", "p": "<b>Ex 8C · Q3(e–g)</b> · Fill in the blanks.", "tag": "", "marks": "", "flat": [{"t": "e) y<sup>2</sup> + 12y + 36: sum (middle term) = __B1__, product = __B2__", "a": {"B1": "12", "B2": "36"}}, {"t": "e) numbers (both, with a comma) = __B1__", "a": {"B1": "6, 6"}, "expr": "list"}, {"t": "e) factors: y<sup>2</sup> + 12y + 36 = (y + 6)(__B1__)", "a": {"B1": "y+6"}, "expr": true}, {"t": "f) x<sup>2</sup> + 5x − 84: sum (middle term) = __B1__, product = __B2__", "a": {"B1": "5", "B2": "-84"}}, {"t": "f) numbers (both, with a comma) = __B1__", "a": {"B1": "-7, 12"}, "expr": "list", "accept": ["12, -7"]}, {"t": "f) factors: x<sup>2</sup> + 5x − 84 = (x − 7)(__B1__)", "a": {"B1": "x+12"}, "expr": true}, {"t": "g) a<sup>2</sup> + 13a − 14: sum (middle term) = __B1__, product = __B2__", "a": {"B1": "13", "B2": "-14"}}, {"t": "g) numbers (both, with a comma) = __B1__", "a": {"B1": "-1, 14"}, "expr": "list", "accept": ["14, -1"]}, {"t": "g) factors: a<sup>2</sup> + 13a − 14 = (a − 1)(__B1__)", "a": {"B1": "a+14"}, "expr": true}], "sol": "e) The middle-term coefficient is 12 and the product is 36.\ne) 6 × 6 = 36 and 6 + 6 = 12.\ne) Product = 36, sum = 12: the numbers are 6 and 6. y<sup>2</sup> + 12y + 36 = y<sup>2</sup> + 6y + 6y + 36 = y(y + 6) + 6(y + 6) = (y + 6)(y + 6) = (y + 6)<sup>2</sup>.\nf) The middle-term coefficient is 5 and the product is −84.\nf) −7 × 12 = −84 and −7 + 12 = 5.\nf) Product = −84, sum = 5: the numbers are −7 and 12. x<sup>2</sup> + 5x − 84 = x<sup>2</sup> − 7x + 12x − 84 = x(x − 7) + 12(x − 7) = (x − 7)(x + 12).\ng) The middle-term coefficient is 13 and the product is −14.\ng) −1 × 14 = −14 and −1 + 14 = 13.\ng) Product = −14, sum = 13: the numbers are −1 and 14. a<sup>2</sup> + 13a − 14 = a<sup>2</sup> − a + 14a − 14 = a(a − 1) + 14(a − 1) = (a − 1)(a + 14)."}, {"kind": "blank", "p": "<b>Ex 8C · Q3(h–j)</b> · Fill in the blanks.", "tag": "", "marks": "", "flat": [{"t": "h) h<sup>2</sup> − 13h − 30: sum (middle term) = __B1__, product = __B2__", "a": {"B1": "-13", "B2": "-30"}}, {"t": "h) numbers (both, with a comma) = __B1__", "a": {"B1": "2, -15"}, "expr": "list", "accept": ["-15, 2"]}, {"t": "h) factors: h<sup>2</sup> − 13h − 30 = (h + 2)(__B1__)", "a": {"B1": "h-15"}, "expr": true}, {"t": "i) y<sup>2</sup> − 3y − 54: sum (middle term) = __B1__, product = __B2__", "a": {"B1": "-3", "B2": "-54"}}, {"t": "i) numbers (both, with a comma) = __B1__", "a": {"B1": "6, -9"}, "expr": "list", "accept": ["-9, 6"]}, {"t": "i) factors: y<sup>2</sup> − 3y − 54 = (y + 6)(__B1__)", "a": {"B1": "y-9"}, "expr": true}, {"t": "j) b<sup>2</sup> − 5b − 24: sum (middle term) = __B1__, product = __B2__", "a": {"B1": "-5", "B2": "-24"}}, {"t": "j) numbers (both, with a comma) = __B1__", "a": {"B1": "3, -8"}, "expr": "list", "accept": ["-8, 3"]}, {"t": "j) factors: b<sup>2</sup> − 5b − 24 = (b + 3)(__B1__)", "a": {"B1": "b-8"}, "expr": true}], "sol": "h) The middle-term coefficient is −13 and the product is −30.\nh) 2 × −15 = −30 and 2 + −15 = −13.\nh) Product = −30, sum = −13: the numbers are 2 and −15. h<sup>2</sup> − 13h − 30 = h<sup>2</sup> + 2h − 15h − 30 = h(h + 2) − 15(h + 2) = (h + 2)(h − 15).\ni) The middle-term coefficient is −3 and the product is −54.\ni) 6 × −9 = −54 and 6 + −9 = −3.\ni) Product = −54, sum = −3: the numbers are 6 and −9. y<sup>2</sup> − 3y − 54 = y<sup>2</sup> + 6y − 9y − 54 = y(y + 6) − 9(y + 6) = (y + 6)(y − 9).\nj) The middle-term coefficient is −5 and the product is −24.\nj) 3 × −8 = −24 and 3 + −8 = −5.\nj) Product = −24, sum = −5: the numbers are 3 and −8. b<sup>2</sup> − 5b − 24 = b<sup>2</sup> + 3b − 8b − 24 = b(b + 3) − 8(b + 3) = (b + 3)(b − 8)."}, {"kind": "mcq", "text": "<b>Ex 8C · Q4(a)</b> · x<sup>2</sup> − 15x + 56 factorised is", "opts": ["(x − 7)(x − 8)", "(x − 4)(x − 14)", "(x + 7)(x + 8)", "(x + 7)(x − 8)"], "correct": 0, "tag": "", "sol": "Product 56, sum −15: −7 and −8. (x − 7)(x − 8)."}, {"kind": "blank", "p": "<b>Ex 8C · Q4(b–e)</b> · Factorise the following. Type the two numbers, the smaller one first (negative numbers with −).", "tag": "", "marks": "", "flat": [{"t": "b) x<sup>2</sup> − 33x + 90 = (x + __B1__)(x + __B2__)", "a": {"B1": "-30", "B2": "-3"}}, {"t": "c) x<sup>2</sup> + 8x + 15 = (x + __B1__)(x + __B2__)", "a": {"B1": "3", "B2": "5"}}, {"t": "d) x<sup>2</sup> − 17x − 60 = (x + __B1__)(x + __B2__)", "a": {"B1": "-20", "B2": "3"}}, {"t": "e) x<sup>2</sup> + 4x − 45 = (x + __B1__)(x + __B2__)", "a": {"B1": "-5", "B2": "9"}}], "sol": "b) Sum −33, product 90: the numbers are −30 and −3. x<sup>2</sup> − 33x + 90 = x<sup>2</sup> − 30x − 3x + 90 = (x − 30)(x − 3).\nc) Sum 8, product 15: the numbers are 3 and 5. x<sup>2</sup> + 8x + 15 = x<sup>2</sup> + 3x + 5x + 15 = (x + 3)(x + 5).\nd) Sum −17, product −60: the numbers are −20 and 3. x<sup>2</sup> − 17x − 60 = x<sup>2</sup> − 20x + 3x − 60 = (x − 20)(x + 3).\ne) Sum 4, product −45: the numbers are −5 and 9. x<sup>2</sup> + 4x − 45 = x<sup>2</sup> − 5x + 9x − 45 = (x − 5)(x + 9)."}, {"kind": "mcq", "text": "<b>Ex 8C · Q4(f)</b> · x<sup>2</sup> + 14x + 45 factorised is", "opts": ["(x + 3)(x + 15)", "(x + 5)(x + 9)", "(x + 5)(x − 9)", "(x − 5)(x − 9)"], "correct": 1, "tag": "", "sol": "Product 45, sum 14: 5 and 9. (x + 5)(x + 9)."}, {"kind": "blank", "p": "<b>Ex 8C · Q4(g, h)</b> · Factorise the following. Type the two numbers, the smaller one first (negative numbers with −).", "tag": "", "marks": "", "flat": [{"t": "g) x<sup>2</sup> − 4x − 21 = (x + __B1__)(x + __B2__)", "a": {"B1": "-7", "B2": "3"}}, {"t": "h) x<sup>2</sup> + 4x − 77 = (x + __B1__)(x + __B2__)", "a": {"B1": "-7", "B2": "11"}}], "sol": "g) Sum −4, product −21: the numbers are −7 and 3. x<sup>2</sup> − 4x − 21 = x<sup>2</sup> − 7x + 3x − 21 = (x − 7)(x + 3).\nh) Sum 4, product −77: the numbers are −7 and 11. x<sup>2</sup> + 4x − 77 = x<sup>2</sup> − 7x + 11x − 77 = (x − 7)(x + 11)."}, {"kind": "blank", "p": "<b>Ex 8C · Q5(a, b)</b> · Factorise. In (a), type the two numbers, the smaller one first.", "tag": "", "marks": "", "flat": [{"t": "a) (x − 2y)<sup>2</sup> + 7(x − 2y) + 12 = (x − 2y + __B1__)(x − 2y + __B2__)", "a": {"B1": "3", "B2": "4"}}, {"t": "b) (3a − b)<sup>2</sup> − 4(3a − b) + 4 = (__B1__)<sup>2</sup>", "a": {"B1": "3a-b-2"}, "expr": true, "accept": ["3a-b-2", "-(3a-b-2)"]}], "sol": "a) Let x − 2y = t: t<sup>2</sup> + 7t + 12 = (t + 3)(t + 4) = (x − 2y + 3)(x − 2y + 4).\nb) Let 3a − b = t: t<sup>2</sup> − 4t + 4 = (t − 2)<sup>2</sup> = (3a − b − 2)<sup>2</sup>."}, {"kind": "mcq", "text": "<b>Ex 8C · Q5(c)</b> · (3x − 4y)<sup>2</sup> + 2(3x − 4y) − 35 factorised is", "opts": ["(3x − 4y − 7)(3x − 4y − 5)", "(3x − 4y + 7)(3x − 4y − 5)", "(3x − 4y − 7)(3x − 4y + 5)", "(3x − 4y + 7)(3x − 4y + 5)"], "correct": 1, "tag": "", "sol": "Let 3x − 4y = t: t<sup>2</sup> + 2t − 35, product −35 and sum 2 → 7 and −5: (t + 7)(t − 5) = (3x − 4y + 7)(3x − 4y − 5)."}, {"kind": "blank", "p": "<b>Ex 8C · Q6</b> · The area of a rectangle is (x<sup>2</sup> + 12x + 35) sq. units. What are its dimensions? Type the longer side first.", "tag": "", "marks": "", "flat": [{"t": "Length × breadth = x<sup>2</sup> + 12x + 35 = (__B1__)(__B2__)", "a": {"B1": "x+7", "B2": "x+5"}, "expr": true}], "sol": "Product 35, sum 12: 5 and 7. x<sup>2</sup> + 12x + 35 = (x + 7)(x + 5): length (x + 7) units, breadth (x + 5) units. (Type the longer side, x + 7, first.)"}]}, {"id": "s6", "label": "8.6 ax² + bx + c", "sub": "Factorising trinomials ax² + bx + c (product = a × c, sum = b)", "slides": [{"kind": "blank", "p": "<b>Example 37</b> · Factorise 2x<sup>2</sup> + 13x + 20. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "Product = (coefficient of x<sup>2</sup>) × (last coefficient) = __B1__", "a": {"B1": "40"}}, {"t": "Two numbers with product 40 and sum 13: __B1__", "a": {"B1": "5, 8"}, "expr": "list", "accept": ["8, 5"]}, {"t": "2x<sup>2</sup> + 13x + 20 = (2x + 5)(__B1__)", "a": {"B1": "x+4"}, "expr": true}], "sol": "2 × 20 = 40.\n5 × 8 = 40 and 5 + 8 = 13.\nProduct = 2 × 20 = 40, sum = 13: the numbers are 5 and 8. 2x<sup>2</sup> + 13x + 20 = 2x<sup>2</sup> + 5x + 8x + 20 = x(2x + 5) + 4(2x + 5) = (2x + 5)(x + 4)."}, {"kind": "mcq", "text": "<b>Example 38</b> · 8a<sup>2</sup> − 22ab + 15b<sup>2</sup> factorised is", "opts": ["(8a − 5b)(a − 3b)", "(4a − 5b)(2a − 3b)", "(4a + 5b)(2a + 3b)", "(4a − 3b)(2a − 5b)"], "correct": 1, "tag": "", "sol": "Product 8 × 15 = 120, sum −22: −10 and −12. 8a<sup>2</sup> − 10ab − 12ab + 15b<sup>2</sup> = 2a(4a − 5b) − 3b(4a − 5b) = (4a − 5b)(2a − 3b)."}, {"kind": "blank", "p": "<b>Example 39</b> · Factorise 10a<sup>2</sup> − 83a − 17. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "Product = (coefficient of a<sup>2</sup>) × (last coefficient) = __B1__", "a": {"B1": "-170"}}, {"t": "Two numbers with product −170 and sum −83: __B1__", "a": {"B1": "2, -85"}, "expr": "list", "accept": ["-85, 2"]}, {"t": "10a<sup>2</sup> − 83a − 17 = (5a + 1)(__B1__)", "a": {"B1": "2a-17"}, "expr": true}], "sol": "10 × (−17) = −170.\n2 × −85 = −170 and 2 + −85 = −83.\nProduct = 10 × (−17) = −170, sum = −83: the numbers are 2 and −85. 10a<sup>2</sup> − 83a − 17 = 10a<sup>2</sup> + 2a − 85a − 17 = 2a(5a + 1) − 17(5a + 1) = (5a + 1)(2a − 17)."}, {"kind": "mcq", "text": "<b>Example 40</b> · 2x<sup>2</sup> − 35x − 18 factorised is", "opts": ["(2x − 9)(x + 2)", "(2x − 18)(x + 1)", "(x + 18)(2x − 1)", "(x − 18)(2x + 1)"], "correct": 3, "tag": "", "sol": "Product 2 × (−18) = −36, sum −35: −36 and 1. 2x<sup>2</sup> − 36x + x − 18 = 2x(x − 18) + 1(x − 18) = (x − 18)(2x + 1)."}, {"kind": "blank", "p": "<b>Try This (p. 105) · (a)</b> · Factorise 3x<sup>2</sup> + x − 4. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "Product = (coefficient of x<sup>2</sup>) × (last coefficient) = __B1__", "a": {"B1": "-12"}}, {"t": "Two numbers with product −12 and sum 1: __B1__", "a": {"B1": "-3, 4"}, "expr": "list", "accept": ["4, -3"]}, {"t": "3x<sup>2</sup> + x − 4 = (x − 1)(__B1__)", "a": {"B1": "3x+4"}, "expr": true}], "sol": "3 × (−4) = −12.\n−3 × 4 = −12 and −3 + 4 = 1.\nProduct = 3 × (−4) = −12, sum = 1: the numbers are −3 and 4. 3x<sup>2</sup> + x − 4 = 3x<sup>2</sup> − 3x + 4x − 4 = 3x(x − 1) + 4(x − 1) = (x − 1)(3x + 4)."}, {"kind": "blank", "p": "<b>Try This (p. 105) · (b)</b> · Factorise 6a<sup>2</sup> − 3ab − 3b<sup>2</sup>. Take out the common number first, then write the bracket with 2a first.", "tag": "", "marks": "", "flat": [{"t": "6a<sup>2</sup> − 3ab − 3b<sup>2</sup> = __B1__(2a<sup>2</sup> − ab − b<sup>2</sup>)", "a": {"B1": "3"}, "expr": true}, {"t": "6a<sup>2</sup> − 3ab − 3b<sup>2</sup> = 3(__B1__)(__B2__)", "a": {"B1": "2a+b", "B2": "a-b"}, "expr": true}], "sol": "3 divides every term: 6a<sup>2</sup> − 3ab − 3b<sup>2</sup> = 3(2a<sup>2</sup> − ab − b<sup>2</sup>).\nFor 2a<sup>2</sup> − ab − b<sup>2</sup>: product 2 × (−1) = −2, sum −1 → −2 and 1. 2a<sup>2</sup> − 2ab + ab − b<sup>2</sup> = 2a(a − b) + b(a − b) = (2a + b)(a − b). Answer: 3(2a + b)(a − b)."}, {"kind": "mcq", "text": "<b>Try This (p. 105) · (c)</b> · 2p<sup>2</sup> + 5p + 3 factorised is", "opts": ["(2p − 3)(p − 1)", "(2p + 1)(p + 3)", "(2p + 3)(p + 1)", "(2p + 3)(p − 1)"], "correct": 2, "tag": "", "sol": "Product 6, sum 5: 2 and 3. 2p<sup>2</sup> + 2p + 3p + 3 = 2p(p + 1) + 3(p + 1) = (p + 1)(2p + 3)."}, {"kind": "blank", "p": "<b>Ex 8D · Q1(a–c)</b> · Factorise. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "a) Two numbers with product 60 and sum 17: __B1__", "a": {"B1": "5, 12"}, "expr": "list", "accept": ["12, 5"]}, {"t": "a) 4x<sup>2</sup> + 17x + 15 = (4x + 5)(__B1__)", "a": {"B1": "x+3"}, "expr": true}, {"t": "b) Two numbers with product 44 and sum −15: __B1__", "a": {"B1": "-4, -11"}, "expr": "list", "accept": ["-11, -4"]}, {"t": "b) 2x<sup>2</sup> − 15x + 22 = (x − 2)(__B1__)", "a": {"B1": "2x-11"}, "expr": true}, {"t": "c) Two numbers with product 120 and sum −22: __B1__", "a": {"B1": "-10, -12"}, "expr": "list", "accept": ["-12, -10"]}, {"t": "c) 8x<sup>2</sup> − 22x + 15 = (4x − 5)(__B1__)", "a": {"B1": "2x-3"}, "expr": true}], "sol": "a) 5 × 12 = 60 and 5 + 12 = 17.\na) Product = 4 × 15 = 60, sum = 17: the numbers are 5 and 12. 4x<sup>2</sup> + 17x + 15 = 4x<sup>2</sup> + 5x + 12x + 15 = x(4x + 5) + 3(4x + 5) = (4x + 5)(x + 3).\nb) −4 × −11 = 44 and −4 + −11 = −15.\nb) Product = 2 × 22 = 44, sum = −15: the numbers are −4 and −11. 2x<sup>2</sup> − 15x + 22 = 2x<sup>2</sup> − 4x − 11x + 22 = 2x(x − 2) − 11(x − 2) = (x − 2)(2x − 11).\nc) −10 × −12 = 120 and −10 + −12 = −22.\nc) Product = 8 × 15 = 120, sum = −22: the numbers are −10 and −12. 8x<sup>2</sup> − 22x + 15 = 8x<sup>2</sup> − 10x − 12x + 15 = 2x(4x − 5) − 3(4x − 5) = (4x − 5)(2x − 3)."}, {"kind": "mcq", "text": "<b>Ex 8D · Q1(d)</b> · 4x<sup>2</sup> − 16x − 9 factorised is", "opts": ["(2x − 9)(2x + 1)", "(4x − 9)(x + 1)", "(2x + 9)(2x − 1)", "(2x − 3)(2x + 3)"], "correct": 0, "tag": "", "sol": "Product 4 × (−9) = −36, sum −16: −18 and 2. 4x<sup>2</sup> − 18x + 2x − 9 = 2x(2x − 9) + 1(2x − 9) = (2x − 9)(2x + 1)."}, {"kind": "blank", "p": "<b>Ex 8D · Q1(e–g)</b> · Factorise. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "e) Two numbers with product 18 and sum 11: __B1__", "a": {"B1": "2, 9"}, "expr": "list", "accept": ["9, 2"]}, {"t": "e) 3x<sup>2</sup> + 11xy + 6y<sup>2</sup> = (3x + 2y)(__B1__)", "a": {"B1": "x+3y"}, "expr": true}, {"t": "f) Two numbers with product −60 and sum −11: __B1__", "a": {"B1": "4, -15"}, "expr": "list", "accept": ["-15, 4"]}, {"t": "f) 6x<sup>2</sup> − 11xy − 10y<sup>2</sup> = (3x + 2y)(__B1__)", "a": {"B1": "2x-5y"}, "expr": true}, {"t": "g) Two numbers with product 4 and sum −4: __B1__", "a": {"B1": "-2, -2"}, "expr": "list"}, {"t": "g) 4x<sup>2</sup> − 4x + 1 = (2x − 1)(__B1__)", "a": {"B1": "2x-1"}, "expr": true}], "sol": "e) 2 × 9 = 18 and 2 + 9 = 11.\ne) Product = 3 × 6 = 18, sum = 11: the numbers are 2 and 9. 3x<sup>2</sup> + 11xy + 6y<sup>2</sup> = 3x<sup>2</sup> + 2xy + 9xy + 6y<sup>2</sup> = x(3x + 2y) + 3y(3x + 2y) = (3x + 2y)(x + 3y).\nf) 4 × −15 = −60 and 4 + −15 = −11.\nf) Product = 6 × (−10) = −60, sum = −11: the numbers are 4 and −15. 6x<sup>2</sup> − 11xy − 10y<sup>2</sup> = 6x<sup>2</sup> + 4xy − 15xy − 10y<sup>2</sup> = 2x(3x + 2y) − 5y(3x + 2y) = (3x + 2y)(2x − 5y).\ng) −2 × −2 = 4 and −2 + −2 = −4.\ng) Product = 4 × 1 = 4, sum = −4: the numbers are −2 and −2. 4x<sup>2</sup> − 4x + 1 = 4x<sup>2</sup> − 2x − 2x + 1 = 2x(2x − 1) − 1(2x − 1) = (2x − 1)(2x − 1) = (2x − 1)<sup>2</sup>."}, {"kind": "mcq", "text": "<b>Ex 8D · Q1(h)</b> · 8x<sup>2</sup> − 21x + 10 factorised is", "opts": ["(8x − 5)(x − 2)", "(8x + 5)(x + 2)", "(8x − 2)(x − 5)", "(4x − 5)(2x − 2)"], "correct": 0, "tag": "", "sol": "Product 80, sum −21: −16 and −5. 8x<sup>2</sup> − 16x − 5x + 10 = 8x(x − 2) − 5(x − 2) = (x − 2)(8x − 5)."}, {"kind": "blank", "p": "<b>Ex 8D · Q1(i–k)</b> · Factorise. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "i) Two numbers with product −30 and sum 7: __B1__", "a": {"B1": "-3, 10"}, "expr": "list", "accept": ["10, -3"]}, {"t": "i) 6a<sup>2</sup> + 7a − 5 = (2a − 1)(__B1__)", "a": {"B1": "3a+5"}, "expr": true}, {"t": "j) Two numbers with product −66 and sum 19: __B1__", "a": {"B1": "-3, 22"}, "expr": "list", "accept": ["22, -3"]}, {"t": "j) 11x<sup>2</sup> + 19x − 6 = (11x − 3)(__B1__)", "a": {"B1": "x+2"}, "expr": true}, {"t": "k) Two numbers with product 25 and sum −10: __B1__", "a": {"B1": "-5, -5"}, "expr": "list"}, {"t": "k) 25x<sup>2</sup> − 10x + 1 = (5x − 1)(__B1__)", "a": {"B1": "5x-1"}, "expr": true}], "sol": "i) −3 × 10 = −30 and −3 + 10 = 7.\ni) Product = 6 × (−5) = −30, sum = 7: the numbers are −3 and 10. 6a<sup>2</sup> + 7a − 5 = 6a<sup>2</sup> − 3a + 10a − 5 = 3a(2a − 1) + 5(2a − 1) = (2a − 1)(3a + 5).\nj) −3 × 22 = −66 and −3 + 22 = 19.\nj) Product = 11 × (−6) = −66, sum = 19: the numbers are −3 and 22. 11x<sup>2</sup> + 19x − 6 = 11x<sup>2</sup> − 3x + 22x − 6 = x(11x − 3) + 2(11x − 3) = (11x − 3)(x + 2).\nk) −5 × −5 = 25 and −5 + −5 = −10.\nk) Product = 25 × 1 = 25, sum = −10: the numbers are −5 and −5. 25x<sup>2</sup> − 10x + 1 = 25x<sup>2</sup> − 5x − 5x + 1 = 5x(5x − 1) − 1(5x − 1) = (5x − 1)(5x − 1) = (5x − 1)<sup>2</sup>."}, {"kind": "mcq", "text": "<b>Ex 8D · Q1(l)</b> · 8x<sup>2</sup> + 13x − 6 factorised is", "opts": ["(8x + 3)(x − 2)", "(4x − 3)(2x + 2)", "(8x − 3)(x + 2)", "(8x − 2)(x + 3)"], "correct": 2, "tag": "", "sol": "Product −48, sum 13: 16 and −3. 8x<sup>2</sup> + 16x − 3x − 6 = 8x(x + 2) − 3(x + 2) = (x + 2)(8x − 3)."}, {"kind": "blank", "p": "<b>Ex 8D · Q1(m–o)</b> · Factorise. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "m) Two numbers with product −28 and sum 3: __B1__", "a": {"B1": "-4, 7"}, "expr": "list", "accept": ["7, -4"]}, {"t": "m) 4x<sup>2</sup> + 3x − 7 = (x − 1)(__B1__)", "a": {"B1": "4x+7"}, "expr": true}, {"t": "n) Two numbers with product 12 and sum −7: __B1__", "a": {"B1": "-3, -4"}, "expr": "list", "accept": ["-4, -3"]}, {"t": "n) 6a<sup>2</sup> − 7a + 2 = (2a − 1)(__B1__)", "a": {"B1": "3a-2"}, "expr": true}, {"t": "o) Two numbers with product 24 and sum 11: __B1__", "a": {"B1": "3, 8"}, "expr": "list", "accept": ["8, 3"]}, {"t": "o) 6 + 11x + 4x<sup>2</sup> = (4x + 3)(__B1__)", "a": {"B1": "x+2"}, "expr": true}], "sol": "m) −4 × 7 = −28 and −4 + 7 = 3.\nm) Product = 4 × (−7) = −28, sum = 3: the numbers are −4 and 7. 4x<sup>2</sup> + 3x − 7 = 4x<sup>2</sup> − 4x + 7x − 7 = 4x(x − 1) + 7(x − 1) = (x − 1)(4x + 7).\nn) −3 × −4 = 12 and −3 + −4 = −7.\nn) Product = 6 × 2 = 12, sum = −7: the numbers are −3 and −4. 6a<sup>2</sup> − 7a + 2 = 6a<sup>2</sup> − 3a − 4a + 2 = 3a(2a − 1) − 2(2a − 1) = (2a − 1)(3a − 2).\no) 3 × 8 = 24 and 3 + 8 = 11.\no) Product = 4 × 6 = 24, sum = 11: the numbers are 3 and 8. 4x<sup>2</sup> + 11x + 6 = 4x<sup>2</sup> + 3x + 8x + 6 = x(4x + 3) + 2(4x + 3) = (4x + 3)(x + 2)."}, {"kind": "mcq", "text": "<b>Ex 8D · Q1(p)</b> · 4x<sup>2</sup> + 8x + 3 factorised is", "opts": ["(2x − 1)(2x − 3)", "(2x + 1)(2x + 3)", "(4x + 3)(x + 1)", "(4x + 1)(x + 3)"], "correct": 1, "tag": "", "sol": "Product 12, sum 8: 2 and 6. 4x<sup>2</sup> + 2x + 6x + 3 = 2x(2x + 1) + 3(2x + 1) = (2x + 1)(2x + 3)."}, {"kind": "blank", "p": "<b>Ex 8D · Q1(q–s)</b> · Factorise. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "q) Two numbers with product 9 and sum −6: __B1__", "a": {"B1": "-3, -3"}, "expr": "list"}, {"t": "q) 9x<sup>2</sup> − 6x + 1 = (3x − 1)(__B1__)", "a": {"B1": "3x-1"}, "expr": true}, {"t": "r) Two numbers with product −9 and sum −8: __B1__", "a": {"B1": "1, -9"}, "expr": "list", "accept": ["-9, 1"]}, {"t": "r) 9x<sup>2</sup> − 8x − 1 = (9x + 1)(__B1__)", "a": {"B1": "x-1"}, "expr": true}, {"t": "s) Two numbers with product −12 and sum −1: __B1__", "a": {"B1": "3, -4"}, "expr": "list", "accept": ["-4, 3"]}, {"t": "s) 3x<sup>2</sup> − x − 4 = (x + 1)(__B1__)", "a": {"B1": "3x-4"}, "expr": true}], "sol": "q) −3 × −3 = 9 and −3 + −3 = −6.\nq) Product = 9 × 1 = 9, sum = −6: the numbers are −3 and −3. 9x<sup>2</sup> − 6x + 1 = 9x<sup>2</sup> − 3x − 3x + 1 = 3x(3x − 1) − 1(3x − 1) = (3x − 1)(3x − 1) = (3x − 1)<sup>2</sup>.\nr) 1 × −9 = −9 and 1 + −9 = −8.\nr) Product = 9 × (−1) = −9, sum = −8: the numbers are 1 and −9. 9x<sup>2</sup> − 8x − 1 = 9x<sup>2</sup> + x − 9x − 1 = x(9x + 1) − 1(9x + 1) = (9x + 1)(x − 1).\ns) 3 × −4 = −12 and 3 + −4 = −1.\ns) Product = 3 × (−4) = −12, sum = −1: the numbers are 3 and −4. 3x<sup>2</sup> − x − 4 = 3x<sup>2</sup> + 3x − 4x − 4 = 3x(x + 1) − 4(x + 1) = (x + 1)(3x − 4)."}, {"kind": "mcq", "text": "<b>Ex 8D · Q1(t)</b> · 5b<sup>2</sup> + 6b − 11 factorised is", "opts": ["(5b + 11)(b − 1)", "(5b − 1)(b + 11)", "(5b − 11)(b + 1)", "(5b + 1)(b − 11)"], "correct": 0, "tag": "", "sol": "Product −55, sum 6: 11 and −5. 5b<sup>2</sup> − 5b + 11b − 11 = 5b(b − 1) + 11(b − 1) = (b − 1)(5b + 11)."}]}, {"id": "s7", "label": "Assessment A", "sub": "Assessment A — Knowing and understanding", "slides": [{"kind": "mcq", "text": "<b>Check-up · MCQ 1</b> · What is the HCF of 20a<sup>2</sup>b<sup>3</sup>c<sup>4</sup>, 8a<sup>2</sup>b<sup>3</sup>c<sup>5</sup> and 12a<sup>3</sup>b<sup>2</sup>c<sup>3</sup>?", "opts": ["4a<sup>2</sup>b<sup>2</sup>c<sup>3</sup>", "8a<sup>3</sup>bc<sup>2</sup>", "12a<sup>3</sup>b<sup>2</sup>c<sup>5</sup>", "12ab<sup>2</sup>c<sup>3</sup>"], "correct": 0, "tag": "", "sol": "HCF(20, 8, 12) = 4; smallest powers: a<sup>2</sup>, b<sup>2</sup>, c<sup>3</sup>. HCF = 4a<sup>2</sup>b<sup>2</sup>c<sup>3</sup>."}, {"kind": "blank", "p": "<b>Check-up · Q3(a–d)</b> · Factorise the following. Take out the HCF (first blank: the HCF, with a positive coefficient; second blank: what is left in the bracket).", "tag": "", "marks": "", "flat": [{"t": "a) 50p + 50q = __B1__(__B2__)", "a": {"B1": "50", "B2": "p+q"}, "expr": true}, {"t": "b) ax − bx = __B1__(__B2__)", "a": {"B1": "x", "B2": "a-b"}, "expr": true}, {"t": "c) x − x<sup>2</sup> = __B1__(__B2__)", "a": {"B1": "x", "B2": "1-x"}, "expr": true}, {"t": "d) 14a<sup>3</sup> − 22a = __B1__(__B2__)", "a": {"B1": "2a", "B2": "7a^2-11"}, "expr": true}], "sol": "a) HCF of the terms = 50. Divide each term by 50: 50p + 50q = 50(p + q).\nb) HCF of the terms = x. Divide each term by x: ax − bx = x(a − b).\nc) HCF of the terms = x. Divide each term by x: x − x<sup>2</sup> = x(1 − x).\nd) HCF of the terms = 2a. Divide each term by 2a: 14a<sup>3</sup> − 22a = 2a(7a<sup>2</sup> − 11)."}, {"kind": "mcq", "text": "<b>Check-up · MCQ 2</b> · The factorised form of (15pq + 15 + 9q + 25p) is", "opts": ["(5p − 3)(3q + 5)", "(5p + 3)(3q + 5)", "(5p − 3)(3q − 5)", "(5p + 3)(3q − 5)"], "correct": 1, "tag": "", "sol": "Regroup: 15pq + 25p + 9q + 15 = 5p(3q + 5) + 3(3q + 5) = (5p + 3)(3q + 5)."}, {"kind": "blank", "p": "<b>Check-up · Q3(e–g)</b> · Factorise the following. Take out the HCF (first blank: the HCF, with a positive coefficient; second blank: what is left in the bracket).", "tag": "", "marks": "", "flat": [{"t": "e) 7x − 14y = __B1__(__B2__)", "a": {"B1": "7", "B2": "x-2y"}, "expr": true}, {"t": "f) 12y<sup>5</sup> − 12y<sup>12</sup> = __B1__(__B2__)", "a": {"B1": "12y^5", "B2": "1-y^7"}, "expr": true}, {"t": "g) 3cd − 3cf − 3ch = __B1__(__B2__)", "a": {"B1": "3c", "B2": "d-f-h"}, "expr": true}], "sol": "e) HCF of the terms = 7. Divide each term by 7: 7x − 14y = 7(x − 2y).\nf) HCF of the terms = 12y<sup>5</sup>. Divide each term by 12y<sup>5</sup>: 12y<sup>5</sup> − 12y<sup>12</sup> = 12y<sup>5</sup>(1 − y<sup>7</sup>).\ng) HCF of the terms = 3c. Divide each term by 3c: 3cd − 3cf − 3ch = 3c(d − f − h)."}, {"kind": "blank", "p": "<b>Check-up · Q4(a)</b> · Find the factors (middle term = sum of the numbers).", "tag": "", "marks": "", "flat": [{"t": "a) t<sup>2</sup> − 25t + 150: sum (middle term) = __B1__, product = __B2__", "a": {"B1": "-25", "B2": "150"}}, {"t": "a) numbers (both, with a comma) = __B1__", "a": {"B1": "-10, -15"}, "expr": "list", "accept": ["-15, -10"]}, {"t": "a) factors: t<sup>2</sup> − 25t + 150 = (t − 10)(__B1__)", "a": {"B1": "t-15"}, "expr": true}, {"t": "b) x<sup>2</sup> − 9xy − 36y<sup>2</sup>: middle term (sum) = __B1__x, product = __B2__", "a": {"B1": "-9y", "B2": "-36y^2"}, "expr": true}, {"t": "b) numbers (the coefficients of y, both, with a comma) = __B1__", "a": {"B1": "-12, 3"}, "expr": "list", "accept": ["3, -12"]}, {"t": "b) factors: x<sup>2</sup> − 9xy − 36y<sup>2</sup> = (x − 12y)(__B1__)", "a": {"B1": "x+3y"}, "expr": true}], "sol": "a) The middle-term coefficient is −25 and the product is 150.\na) −10 × −15 = 150 and −10 + −15 = −25.\na) Product = 150, sum = −25: the numbers are −10 and −15. t<sup>2</sup> − 25t + 150 = t<sup>2</sup> − 10t − 15t + 150 = t(t − 10) − 15(t − 10) = (t − 10)(t − 15).\nb) The middle term is −9xy = (−9y)x and the product is −36y<sup>2</sup>.\nb) −12y × 3y = −36y<sup>2</sup> and −12y + 3y = −9y.\nb) x<sup>2</sup> − 12xy + 3xy − 36y<sup>2</sup> = x(x − 12y) + 3y(x − 12y) = (x − 12y)(x + 3y)."}, {"kind": "mcq", "text": "To factorise x<sup>2</sup> + bx + c when c is <b>negative</b>, the two numbers", "opts": ["have opposite signs", "are both positive", "are always equal", "are both negative"], "correct": 0, "tag": "", "sol": "A negative product needs one positive and one negative number; the sign of b tells which one is bigger."}, {"kind": "blank", "p": "<b>Check-up · Q4(c–e)</b> · Find the factors (middle term = sum of the numbers).", "tag": "", "marks": "", "flat": [{"t": "c) m<sup>2</sup> − m − 90: sum (middle term) = __B1__, product = __B2__", "a": {"B1": "-1", "B2": "-90"}}, {"t": "c) numbers (both, with a comma) = __B1__", "a": {"B1": "9, -10"}, "expr": "list", "accept": ["-10, 9"]}, {"t": "c) factors: m<sup>2</sup> − m − 90 = (m + 9)(__B1__)", "a": {"B1": "m-10"}, "expr": true}, {"t": "d) a<sup>2</sup> − 5a − 66: sum (middle term) = __B1__, product = __B2__", "a": {"B1": "-5", "B2": "-66"}}, {"t": "d) numbers (both, with a comma) = __B1__", "a": {"B1": "6, -11"}, "expr": "list", "accept": ["-11, 6"]}, {"t": "d) factors: a<sup>2</sup> − 5a − 66 = (a + 6)(__B1__)", "a": {"B1": "a-11"}, "expr": true}, {"t": "e) x<sup>2</sup> − 17x + 70: sum (middle term) = __B1__, product = __B2__", "a": {"B1": "-17", "B2": "70"}}, {"t": "e) numbers (both, with a comma) = __B1__", "a": {"B1": "-7, -10"}, "expr": "list", "accept": ["-10, -7"]}, {"t": "e) factors: x<sup>2</sup> − 17x + 70 = (x − 7)(__B1__)", "a": {"B1": "x-10"}, "expr": true}], "sol": "c) The middle-term coefficient is −1 and the product is −90.\nc) 9 × −10 = −90 and 9 + −10 = −1.\nc) Product = −90, sum = −1: the numbers are 9 and −10. m<sup>2</sup> − m − 90 = m<sup>2</sup> + 9m − 10m − 90 = m(m + 9) − 10(m + 9) = (m + 9)(m − 10).\nd) The middle-term coefficient is −5 and the product is −66.\nd) 6 × −11 = −66 and 6 + −11 = −5.\nd) Product = −66, sum = −5: the numbers are 6 and −11. a<sup>2</sup> − 5a − 66 = a<sup>2</sup> + 6a − 11a − 66 = a(a + 6) − 11(a + 6) = (a + 6)(a − 11).\ne) The middle-term coefficient is −17 and the product is 70.\ne) −7 × −10 = 70 and −7 + −10 = −17.\ne) Product = 70, sum = −17: the numbers are −7 and −10. x<sup>2</sup> − 17x + 70 = x<sup>2</sup> − 7x − 10x + 70 = x(x − 7) − 10(x − 7) = (x − 7)(x − 10)."}, {"kind": "mcq", "text": "Which of these is <b>not</b> completely factorised?", "opts": ["3x(x + 2)", "2(2x + 3)(2x − 3)", "2(4x<sup>2</sup> − 9)", "(x + 1)(x − 1)"], "correct": 2, "tag": "", "sol": "4x<sup>2</sup> − 9 = (2x + 3)(2x − 3) can still be split, so 2(4x<sup>2</sup> − 9) is not complete."}]}, {"id": "s8", "label": "Assessment B", "sub": "Assessment B — Investigating patterns", "slides": [{"kind": "blank", "p": "<b>Check-up · Q5(a–d)</b> · Factorise the following (spot the perfect squares).", "tag": "", "marks": "", "flat": [{"t": "a) 4a<sup>2</sup> − 4a + 1 = (__B1__)<sup>2</sup>", "a": {"B1": "2a-1"}, "expr": true, "accept": ["2a-1", "-(2a-1)"]}, {"t": "b) m<sup>2</sup> − 4m + 4 = (__B1__)<sup>2</sup>", "a": {"B1": "m-2"}, "expr": true, "accept": ["m-2", "-(m-2)"]}, {"t": "c) m<sup>2</sup> + 14m + 49 = (__B1__)<sup>2</sup>", "a": {"B1": "m+7"}, "expr": true, "accept": ["m+7", "-(m+7)"]}, {"t": "d) 9x<sup>2</sup> − 30x + 25 = (__B1__)<sup>2</sup>", "a": {"B1": "3x-5"}, "expr": true, "accept": ["3x-5", "-(3x-5)"]}], "sol": "a) (2a)<sup>2</sup> − 2(2a)(1) + 1 = (2a − 1)<sup>2</sup>.\nb) (m − 2)<sup>2</sup>.\nc) 14m = 2 × m × 7: (m + 7)<sup>2</sup>.\nd) (3x)<sup>2</sup> − 2(3x)(5) + 5<sup>2</sup> = (3x − 5)<sup>2</sup>."}, {"kind": "mcq", "text": "Which expression is a perfect square?", "opts": ["x<sup>2</sup> − 10x − 25", "x<sup>2</sup> − 10x + 25", "x<sup>2</sup> + 10x − 25", "x<sup>2</sup> + 5x + 25"], "correct": 1, "tag": "", "sol": "a<sup>2</sup> − 2ab + b<sup>2</sup> with a = x, b = 5: x<sup>2</sup> − 10x + 25 = (x − 5)<sup>2</sup>. A perfect square needs a + sign on b<sup>2</sup> and a middle term of 2ab."}, {"kind": "blank", "p": "<b>Check-up · Q5(e, f)</b> · Factorise completely.", "tag": "", "marks": "", "flat": [{"t": "e) x<sup>4</sup> − y<sup>4</sup> (write x<sup>2</sup> + y<sup>2</sup> first, then x + y, then x − y) = (__B1__)(__B2__)(__B3__)", "a": {"B1": "x^2+y^2", "B2": "x+y", "B3": "x-y"}, "expr": true}, {"t": "f) x<sup>4</sup> − 16 = (x<sup>2</sup> + __B1__)(x + __B2__)(x − __B3__)", "a": {"B1": "4", "B2": "2", "B3": "2"}, "expr": true}], "sol": "e) (x<sup>2</sup>)<sup>2</sup> − (y<sup>2</sup>)<sup>2</sup> = (x<sup>2</sup> + y<sup>2</sup>)(x<sup>2</sup> − y<sup>2</sup>) = (x<sup>2</sup> + y<sup>2</sup>)(x + y)(x − y).\nf) (x<sup>2</sup> + 4)(x<sup>2</sup> − 4) = (x<sup>2</sup> + 4)(x + 2)(x − 2)."}, {"kind": "mcq", "text": "<b>Check-up · Q5(g)</b> · k<sup>2</sup> − {81/121} factorised is", "opts": ["(k + {9/11})(k − {9/11})", "(k + {9/121})(k − {9/121})", "(k − {9/11})<sup>2</sup>", "(k + {81/11})(k − {81/11})"], "correct": 0, "tag": "", "sol": "{81/121} = ({9/11})<sup>2</sup>: (k + {9/11})(k − {9/11})."}, {"kind": "blank", "p": "<b>Check-up · Q5(h, i)</b> · Factorise. In (i), write the bracket with + first. Type fractions like a/2.", "tag": "", "marks": "", "flat": [{"t": "h) 169a<sup>2</sup> − 52ab + 4b<sup>2</sup> = (__B1__)<sup>2</sup>", "a": {"B1": "13a-2b"}, "expr": true, "accept": ["13a-2b", "-(13a-2b)"]}, {"t": "i) a<sup>2</sup>/4 − b<sup>2</sup>/25 = (__B1__)(__B2__)", "a": {"B1": "a/2+b/5", "B2": "a/2-b/5"}, "expr": true}], "sol": "h) (13a)<sup>2</sup> − 2(13a)(2b) + (2b)<sup>2</sup> = (13a − 2b)<sup>2</sup>.\ni) ({a/2})<sup>2</sup> − ({b/5})<sup>2</sup> = ({a/2} + {b/5})({a/2} − {b/5})."}, {"kind": "mcq", "text": "<b>Check-up · Q5(j)</b> · (a − 5)<sup>2</sup> − (b − 8)<sup>2</sup> factorised is", "opts": ["(a + b − 13)(a − b + 3)", "(a + b − 3)(a − b + 13)", "(a − b − 3)(a + b − 13)", "(a − b + 3)(a − b − 13)"], "correct": 0, "tag": "", "sol": "[(a − 5) + (b − 8)][(a − 5) − (b − 8)] = (a + b − 13)(a − b + 3)."}, {"kind": "blank", "p": "<b>Check-up · Q5(k, l)</b> · Factorise completely. In (k), first blank: the HCF; then write the bracket with + first.", "tag": "", "marks": "", "flat": [{"t": "k) 27m<sup>7</sup> − 48m = __B1__(__B2__)(__B3__)", "a": {"B1": "3m", "B2": "3m^3+4", "B3": "3m^3-4"}, "expr": true}, {"t": "l) 9a<sup>2</sup> + 12ab + 4b<sup>2</sup> = (__B1__)<sup>2</sup>", "a": {"B1": "3a+2b"}, "expr": true, "accept": ["3a+2b", "-(3a+2b)"]}, {"t": "l) 81 − 9a<sup>2</sup> − 12ab − 4b<sup>2</sup> = (9 + 3a + 2b)(__B1__)", "a": {"B1": "9-3a-2b"}, "expr": true}], "sol": "k) 27m<sup>7</sup> − 48m = 3m(9m<sup>6</sup> − 16) = 3m(3m<sup>3</sup> + 4)(3m<sup>3</sup> − 4).\nl) 81 − 9a<sup>2</sup> − 12ab − 4b<sup>2</sup> = 81 − (9a<sup>2</sup> + 12ab + 4b<sup>2</sup>) = 9<sup>2</sup> − (3a + 2b)<sup>2</sup>.\nl) (9 + 3a + 2b)(9 − 3a − 2b)."}, {"kind": "mcq", "text": "<b>Check-up · HOTS</b> · If the sum of (x + 9) observations is (x<sup>4</sup> − 6561), what is the mean of the observations?", "opts": ["(x + 9)(x<sup>2</sup> + 81)", "(x − 9)(x<sup>2</sup> + 81)", "(x − 9)(x<sup>2</sup> − 81)", "(x − 9)<sup>2</sup>(x + 9)"], "correct": 1, "tag": "", "sol": "Mean = sum ÷ number = (x<sup>4</sup> − 6561) ÷ (x + 9). x<sup>4</sup> − 6561 = (x<sup>2</sup> + 81)(x<sup>2</sup> − 81) = (x<sup>2</sup> + 81)(x + 9)(x − 9), so the mean is (x − 9)(x<sup>2</sup> + 81)."}, {"kind": "blank", "p": "<b>Check-up · Q6(a–h)</b> · Factorise the following expressions. Type the two numbers, the smaller one first (negative numbers with −).", "tag": "", "marks": "", "flat": [{"t": "a) x<sup>2</sup> − 6x − 72 = (x + __B1__)(x + __B2__)", "a": {"B1": "-12", "B2": "6"}}, {"t": "b) x<sup>2</sup> − 25x + 150 = (x + __B1__)(x + __B2__)", "a": {"B1": "-15", "B2": "-10"}}, {"t": "c) x<sup>2</sup> − 5x − 300 = (x + __B1__)(x + __B2__)", "a": {"B1": "-20", "B2": "15"}}, {"t": "d) x<sup>2</sup> + 15x + 54 = (x + __B1__)(x + __B2__)", "a": {"B1": "6", "B2": "9"}}, {"t": "e) x<sup>2</sup> − 18x + 80 = (x + __B1__)(x + __B2__)", "a": {"B1": "-10", "B2": "-8"}}, {"t": "f) x<sup>2</sup> + 2x − 15 = (x + __B1__)(x + __B2__)", "a": {"B1": "-3", "B2": "5"}}, {"t": "g) x<sup>2</sup> − 4x − 77 = (x + __B1__)(x + __B2__)", "a": {"B1": "-11", "B2": "7"}}, {"t": "h) x<sup>2</sup> − 11x − 26 = (x + __B1__)(x + __B2__)", "a": {"B1": "-13", "B2": "2"}}], "sol": "a) Sum −6, product −72: the numbers are −12 and 6. x<sup>2</sup> − 6x − 72 = x<sup>2</sup> − 12x + 6x − 72 = (x − 12)(x + 6).\nb) Sum −25, product 150: the numbers are −15 and −10. x<sup>2</sup> − 25x + 150 = x<sup>2</sup> − 15x − 10x + 150 = (x − 15)(x − 10).\nc) Sum −5, product −300: the numbers are −20 and 15. x<sup>2</sup> − 5x − 300 = x<sup>2</sup> − 20x + 15x − 300 = (x − 20)(x + 15).\nd) Sum 15, product 54: the numbers are 6 and 9. x<sup>2</sup> + 15x + 54 = x<sup>2</sup> + 6x + 9x + 54 = (x + 6)(x + 9).\ne) Sum −18, product 80: the numbers are −10 and −8. x<sup>2</sup> − 18x + 80 = x<sup>2</sup> − 10x − 8x + 80 = (x − 10)(x − 8).\nf) Sum 2, product −15: the numbers are −3 and 5. x<sup>2</sup> + 2x − 15 = x<sup>2</sup> − 3x + 5x − 15 = (x − 3)(x + 5).\ng) Sum −4, product −77: the numbers are −11 and 7. x<sup>2</sup> − 4x − 77 = x<sup>2</sup> − 11x + 7x − 77 = (x − 11)(x + 7).\nh) Sum −11, product −26: the numbers are −13 and 2. x<sup>2</sup> − 11x − 26 = x<sup>2</sup> − 13x + 2x − 26 = (x − 13)(x + 2)."}, {"kind": "blank", "p": "<b>Check-up · Q6(i–o)</b> · Factorise the following expressions. Type the two numbers, the smaller one first (negative numbers with −).", "tag": "", "marks": "", "flat": [{"t": "i) x<sup>2</sup> − 16x + 39 = (x + __B1__)(x + __B2__)", "a": {"B1": "-13", "B2": "-3"}}, {"t": "j) x<sup>2</sup> + 20x + 36 = (x + __B1__)(x + __B2__)", "a": {"B1": "2", "B2": "18"}}, {"t": "k) x<sup>2</sup> − 10x − 24 = (x + __B1__)(x + __B2__)", "a": {"B1": "-12", "B2": "2"}}, {"t": "l) x<sup>2</sup> − 2x − 8 = (x + __B1__)(x + __B2__)", "a": {"B1": "-4", "B2": "2"}}, {"t": "m) x<sup>2</sup> − 30x − 99 = (x + __B1__)(x + __B2__)", "a": {"B1": "-33", "B2": "3"}}, {"t": "n) x<sup>2</sup> + x − 90 = (x + __B1__)(x + __B2__)", "a": {"B1": "-9", "B2": "10"}}, {"t": "o) p<sup>2</sup> − 31p − 32 = (p + __B1__)(p + __B2__)", "a": {"B1": "-32", "B2": "1"}}], "sol": "i) Sum −16, product 39: the numbers are −13 and −3. x<sup>2</sup> − 16x + 39 = x<sup>2</sup> − 13x − 3x + 39 = (x − 13)(x − 3).\nj) Sum 20, product 36: the numbers are 2 and 18. x<sup>2</sup> + 20x + 36 = x<sup>2</sup> + 2x + 18x + 36 = (x + 2)(x + 18).\nk) Sum −10, product −24: the numbers are −12 and 2. x<sup>2</sup> − 10x − 24 = x<sup>2</sup> − 12x + 2x − 24 = (x − 12)(x + 2).\nl) Sum −2, product −8: the numbers are −4 and 2. x<sup>2</sup> − 2x − 8 = x<sup>2</sup> − 4x + 2x − 8 = (x − 4)(x + 2).\nm) Sum −30, product −99: the numbers are −33 and 3. x<sup>2</sup> − 30x − 99 = x<sup>2</sup> − 33x + 3x − 99 = (x − 33)(x + 3).\nn) Sum 1, product −90: the numbers are −9 and 10. x<sup>2</sup> + x − 90 = x<sup>2</sup> − 9x + 10x − 90 = (x − 9)(x + 10).\no) Sum −31, product −32: the numbers are −32 and 1. p<sup>2</sup> − 31p − 32 = p<sup>2</sup> − 32p + 1p − 32 = (p − 32)(p + 1)."}]}, {"id": "s9", "label": "Assessment C", "sub": "Assessment C — Communicating", "slides": [{"kind": "blank", "p": "<b>Check-up · Q7(a, b)</b> · Factorise. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "a) Two numbers with product 60 and sum −32: __B1__", "a": {"B1": "-2, -30"}, "expr": "list", "accept": ["-30, -2"]}, {"t": "a) 5x<sup>2</sup> − 32xy + 12y<sup>2</sup> = (5x − 2y)(__B1__)", "a": {"B1": "x-6y"}, "expr": true}, {"t": "b) Two numbers with product −24 and sum 5: __B1__", "a": {"B1": "-3, 8"}, "expr": "list", "accept": ["8, -3"]}, {"t": "b) 2x<sup>2</sup> + 5xy − 12y<sup>2</sup> = (2x − 3y)(__B1__)", "a": {"B1": "x+4y"}, "expr": true}], "sol": "a) −2 × −30 = 60 and −2 + −30 = −32.\na) Product = 5 × 12 = 60, sum = −32: the numbers are −2 and −30. 5x<sup>2</sup> − 32xy + 12y<sup>2</sup> = 5x<sup>2</sup> − 2xy − 30xy + 12y<sup>2</sup> = x(5x − 2y) − 6y(5x − 2y) = (5x − 2y)(x − 6y).\nb) −3 × 8 = −24 and −3 + 8 = 5.\nb) Product = 2 × (−12) = −24, sum = 5: the numbers are −3 and 8. 2x<sup>2</sup> + 5xy − 12y<sup>2</sup> = 2x<sup>2</sup> − 3xy + 8xy − 12y<sup>2</sup> = x(2x − 3y) + 4y(2x − 3y) = (2x − 3y)(x + 4y)."}, {"kind": "mcq", "text": "Rohit wrote x<sup>2</sup> − 5x − 14 = (x + 7)(x − 2). What is wrong?", "opts": ["7 × (−2) = −14 is wrong, so no pair of numbers works here", "7 + (−2) = +5, but the middle term is −5x: it should be (x − 7)(x + 2)", "He should have taken out the HCF 7 before splitting the middle term", "Nothing: (x + 7)(x − 2) expands to x<sup>2</sup> − 5x − 14"], "correct": 1, "tag": "", "sol": "(x + 7)(x − 2) = x<sup>2</sup> + 5x − 14. The numbers need sum −5 and product −14: −7 and 2, giving (x − 7)(x + 2)."}, {"kind": "blank", "p": "<b>Check-up · Q7(c, d)</b> · Factorise. Find the two numbers (type both, in any order, separated by a comma), then complete the factorisation.", "tag": "", "marks": "", "flat": [{"t": "c) Two numbers with product −170 and sum −83: __B1__", "a": {"B1": "2, -85"}, "expr": "list", "accept": ["-85, 2"]}, {"t": "c) 10a<sup>2</sup> − 83a − 17 = (5a + 1)(__B1__)", "a": {"B1": "2a-17"}, "expr": true}, {"t": "d) Two numbers with product 40 and sum 13: __B1__", "a": {"B1": "5, 8"}, "expr": "list", "accept": ["8, 5"]}, {"t": "d) 4x<sup>2</sup> + 13xy + 10y<sup>2</sup> = (4x + 5y)(__B1__)", "a": {"B1": "x+2y"}, "expr": true}], "sol": "c) 2 × −85 = −170 and 2 + −85 = −83.\nc) Product = 10 × (−17) = −170, sum = −83: the numbers are 2 and −85. 10a<sup>2</sup> − 83a − 17 = 10a<sup>2</sup> + 2a − 85a − 17 = 2a(5a + 1) − 17(5a + 1) = (5a + 1)(2a − 17).\nd) 5 × 8 = 40 and 5 + 8 = 13.\nd) Product = 4 × 10 = 40, sum = 13: the numbers are 5 and 8. 4x<sup>2</sup> + 13xy + 10y<sup>2</sup> = 4x<sup>2</sup> + 5xy + 8xy + 10y<sup>2</sup> = x(4x + 5y) + 2y(4x + 5y) = (4x + 5y)(x + 2y)."}, {"kind": "mcq", "text": "Why should you take out the HCF first when factorising 2x<sup>2</sup> − 18?", "opts": ["2x<sup>2</sup> − 18 has no factors unless the HCF is removed first", "2(x<sup>2</sup> − 9) shows the identity a<sup>2</sup> − b<sup>2</sup>, giving 2(x + 3)(x − 3)", "The HCF must always be the last factor written in the answer", "Taking out 2 changes the value, so the answer becomes easier"], "correct": 1, "tag": "", "sol": "2x<sup>2</sup> and 18 are not perfect squares, but after taking out 2 the bracket x<sup>2</sup> − 9 = (x + 3)(x − 3)."}, {"kind": "blank", "p": "<b>Check-up · Q8(a–e)</b> · Factorise the following. For two numbers, type the smaller one first.", "tag": "", "marks": "", "flat": [{"t": "a) 1 − 2x + x<sup>2</sup> = (__B1__)<sup>2</sup>", "a": {"B1": "1-x"}, "expr": true, "accept": ["1-x", "-(1-x)"]}, {"t": "b) 24 + 11q + q<sup>2</sup> = (q + __B1__)(q + __B2__)", "a": {"B1": "3", "B2": "8"}}, {"t": "c) x<sup>2</sup> − 30x + 225 = (__B1__)<sup>2</sup>", "a": {"B1": "x-15"}, "expr": true, "accept": ["x-15", "-(x-15)"]}, {"t": "d) x<sup>2</sup> + 8x − 65 = (x + __B1__)(x + __B2__)", "a": {"B1": "-5", "B2": "13"}}, {"t": "e) x<sup>2</sup> + 19x − 66 = (x + __B1__)(x + __B2__)", "a": {"B1": "-3", "B2": "22"}}], "sol": "a) 1 − 2(1)(x) + x<sup>2</sup> = (1 − x)<sup>2</sup>.\nb) Sum 11, product 24: the numbers are 3 and 8. 24 + 11q + q<sup>2</sup> = q<sup>2</sup> + 3q + 8q + 24 = (q + 3)(q + 8).\nc) 225 = 15<sup>2</sup>, 30x = 2 × x × 15: (x − 15)<sup>2</sup>.\nd) Sum 8, product −65: the numbers are −5 and 13. x<sup>2</sup> + 8x − 65 = x<sup>2</sup> − 5x + 13x − 65 = (x − 5)(x + 13).\ne) Sum 19, product −66: the numbers are −3 and 22. x<sup>2</sup> + 19x − 66 = x<sup>2</sup> − 3x + 22x − 66 = (x − 3)(x + 22)."}, {"kind": "blank", "p": "<b>Check-up · Q8(f–h)</b> · Factorise the following. In (f), write the bracket with + first.", "tag": "", "marks": "", "flat": [{"t": "f) 16x<sup>2</sup> − 25 = (__B1__)(__B2__)", "a": {"B1": "4x+5", "B2": "4x-5"}, "expr": true}, {"t": "g) Two numbers with product −24 and sum −10: __B1__", "a": {"B1": "2, -12"}, "expr": "list", "accept": ["-12, 2"]}, {"t": "g) 8x<sup>2</sup> − 10x − 3 = (4x + 1)(__B1__)", "a": {"B1": "2x-3"}, "expr": true}, {"t": "h) Two numbers with product 120 and sum 22: __B1__", "a": {"B1": "10, 12"}, "expr": "list", "accept": ["12, 10"]}, {"t": "h) 8x<sup>2</sup> + 22x + 15 = (4x + 5)(__B1__)", "a": {"B1": "2x+3"}, "expr": true}], "sol": "f) (4x)<sup>2</sup> − 5<sup>2</sup> = (4x + 5)(4x − 5).\ng) 2 × −12 = −24 and 2 + −12 = −10.\ng) Product = 8 × (−3) = −24, sum = −10: the numbers are 2 and −12. 8x<sup>2</sup> − 10x − 3 = 8x<sup>2</sup> + 2x − 12x − 3 = 2x(4x + 1) − 3(4x + 1) = (4x + 1)(2x − 3).\nh) 10 × 12 = 120 and 10 + 12 = 22.\nh) Product = 8 × 15 = 120, sum = 22: the numbers are 10 and 12. 8x<sup>2</sup> + 22x + 15 = 8x<sup>2</sup> + 10x + 12x + 15 = 2x(4x + 5) + 3(4x + 5) = (4x + 5)(2x + 3)."}, {"kind": "mcq", "text": "After factorising an expression, the best way to check your answer is to", "opts": ["put x = 0 only, since one value is always enough", "add the factors and compare with the original expression", "multiply the factors out and compare with the original expression", "check that every factor has the same number of terms"], "correct": 2, "tag": "", "sol": "Factorising is the reverse of multiplying, so expanding the product must give back the original expression exactly (x = 0 alone can miss mistakes in the x terms)."}, {"kind": "blank", "p": "<b>Check-up · Q8(i, j)</b> · Factorise the following.", "tag": "", "marks": "", "flat": [{"t": "i) (4p + 5q)<sup>2</sup> + 8(4p + 5q) + 16 = (__B1__)<sup>2</sup>", "a": {"B1": "4p+5q+4"}, "expr": true, "accept": ["4p+5q+4", "-(4p+5q+4)"]}, {"t": "j) (3x − y)<sup>2</sup> − 4(3x − y) + 4 = (__B1__)<sup>2</sup>", "a": {"B1": "3x-y-2"}, "expr": true, "accept": ["3x-y-2", "-(3x-y-2)"]}], "sol": "i) Let t = 4p + 5q: t<sup>2</sup> + 8t + 16 = (t + 4)<sup>2</sup> = (4p + 5q + 4)<sup>2</sup>.\nj) Let t = 3x − y: t<sup>2</sup> − 4t + 4 = (t − 2)<sup>2</sup> = (3x − y − 2)<sup>2</sup>."}, {"kind": "mcq", "text": "Asha wrote 4x<sup>2</sup> − 9 = (4x + 3)(4x − 3). Which statement is correct?", "opts": ["4x<sup>2</sup> − 9 cannot be factorised because 4 is not 9", "4x<sup>2</sup> = (2x)<sup>2</sup>, so the answer is (2x + 3)(2x − 3)", "The answer is (4x + 9)(4x − 9), since 9 is the last term", "Her answer is correct because 4x × 4x = 4x<sup>2</sup>"], "correct": 1, "tag": "", "sol": "(4x + 3)(4x − 3) = 16x<sup>2</sup> − 9. The square root of 4x<sup>2</sup> is 2x, so 4x<sup>2</sup> − 9 = (2x + 3)(2x − 3)."}]}, {"id": "s10", "label": "Assessment D", "sub": "Assessment D — Applying mathematics in real-life contexts", "slides": [{"kind": "mcq", "text": "<b>Check-up · Case study Q9(a)</b> · Mahira got a scratch-and-win ticket on purchasing some groceries from a supermarket. She won some cash on the ticket and she also won ₹70 as a bonus. At the cash counter she was told she had won a ‘triple your winnings’ ticket because her bill amount was more than ₹2500. She was also the 100th customer of the day, which automatically doubled her total winnings. If Mahira won ₹x through the scratch-and-win ticket, then the total amount won by her can be expressed as ₹ ___.", "opts": ["6 + x + 70", "None of these", "2(x + 70)", "6(x + 70)"], "correct": 3, "tag": "", "sol": "Winnings x + 70; tripled: 3(x + 70); doubled: 2 × 3(x + 70) = 6(x + 70)."}, {"kind": "mcq", "text": "<b>Check-up · Case study Q9(b)</b> · In the same situation, if Mahira’s bill amount was less than ₹2500, then she would have won ₹ ___.", "opts": ["x + 70", "2x + 70", "None of these", "2(x + 70)"], "correct": 3, "tag": "", "sol": "No tripling, but being the 100th customer still doubles the total: 2(x + 70)."}, {"kind": "blank", "p": "<b>Check-up · Everyday Maths Q10</b> · If Shalini spent an amount of ₹(ac<sup>2</sup> + 4ab<sup>2</sup> − 4abc) for buying (c − 2b) same-priced shirts, find the price of each shirt.", "tag": "", "marks": "", "flat": [{"t": "ac<sup>2</sup> + 4ab<sup>2</sup> − 4abc = a(__B1__)<sup>2</sup>", "a": {"B1": "c-2b"}, "expr": true, "accept": ["c-2b", "-(c-2b)"]}, {"t": "Price of each shirt = ₹__B1__", "a": {"B1": "a(c-2b)"}, "expr": true}], "sol": "HCF a: a(c<sup>2</sup> − 4bc + 4b<sup>2</sup>) = a(c − 2b)<sup>2</sup>.\nPrice = a(c − 2b)<sup>2</sup> ÷ (c − 2b) = ₹a(c − 2b)."}, {"kind": "blank", "p": "<b>Check-up · Everyday Maths Q11</b> · To make a rectangular lawn in front of the school, the principal spent ₹(10x<sup>2</sup> + 35x + 30) at the rate of ₹5 per square metre. What is the area of the lawn? What are its dimensions? (Type the side with 2x first.)", "tag": "", "marks": "", "flat": [{"t": "Area = __B1__ m<sup>2</sup>", "a": {"B1": "2x^2+7x+6"}, "expr": true}, {"t": "Dimensions: 2x<sup>2</sup> + 7x + 6 = (__B1__)(__B2__)", "a": {"B1": "2x+3", "B2": "x+2"}, "expr": true}], "sol": "Area = cost ÷ rate = (10x<sup>2</sup> + 35x + 30) ÷ 5 = 2x<sup>2</sup> + 7x + 6 m<sup>2</sup>.\nProduct 12, sum 7: 3 and 4. 2x<sup>2</sup> + 4x + 3x + 6 = 2x(x + 2) + 3(x + 2) = (2x + 3)(x + 2): the lawn is (2x + 3) m by (x + 2) m."}, {"kind": "mcq", "text": "<b>Maths Lab Activity</b> · A square of side x and five strips of x × 1 are placed side by side to form a rectangle (x + 5) long and x wide. What does the rectangle show?", "opts": ["x(x + 5) = 6x<sup>2</sup>", "x(x + 5) = 2x + 5", "x(x + 5) = x<sup>2</sup> + 5", "x(x + 5) = x<sup>2</sup> + 5x"], "correct": 3, "tag": "", "sol": "The square has area x<sup>2</sup> and each strip has area x × 1 = x, so the rectangle has area x<sup>2</sup> + 5x: x(x + 5) = x<sup>2</sup> + 5x.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 254 136\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><rect class=\"sh\" x=\"34\" y=\"26\" width=\"96\" height=\"96\"/><rect class=\"sh3\" x=\"130\" y=\"26\" width=\"18\" height=\"96\"/><rect class=\"sh3\" x=\"148\" y=\"26\" width=\"18\" height=\"96\"/><rect class=\"sh3\" x=\"166\" y=\"26\" width=\"18\" height=\"96\"/><rect class=\"sh3\" x=\"184\" y=\"26\" width=\"18\" height=\"96\"/><rect class=\"sh3\" x=\"202\" y=\"26\" width=\"18\" height=\"96\"/><text class=\"lb\" x=\"82.0\" y=\"14.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">x</text><text class=\"lb\" x=\"175.0\" y=\"14.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">5</text><text class=\"lb\" x=\"20.0\" y=\"74.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">x</text><line class=\"tick\" x1=\"34.0\" y1=\"22.0\" x2=\"130.0\" y2=\"22.0\"/><line class=\"tick\" x1=\"130.0\" y1=\"22.0\" x2=\"220.0\" y2=\"22.0\"/><line class=\"tick\" x1=\"30.0\" y1=\"26.0\" x2=\"30.0\" y2=\"122.0\"/></svg>"}, {"kind": "blank", "p": "<b>Check-up · Brain Teaser</b> · The length of a piece of rectangular floor tile is 5 cm more than twice the side x cm of a second square piece of tile. The width of the rectangular piece is 2 cm less than twice the side of the square piece. Find the area of the rectangular piece in terms of x (in expanded form).", "tag": "", "marks": "", "flat": [{"t": "Length = __B1__ cm", "a": {"B1": "2x+5"}, "expr": true}, {"t": "Width = __B1__ cm", "a": {"B1": "2x-2"}, "expr": true}, {"t": "Area = __B1__x<sup>2</sup> + __B2__x + (__B3__) cm<sup>2</sup>", "a": {"B1": "4", "B2": "6", "B3": "-10"}}], "sol": "Twice the side plus 5: 2x + 5.\nTwice the side minus 2: 2x − 2.\n(2x + 5)(2x − 2) = 4x<sup>2</sup> − 4x + 10x − 10 = 4x<sup>2</sup> + 6x − 10 cm<sup>2</sup>."}, {"kind": "blank", "p": "<b>Maths Lab Activity</b> · The figure shows (x + 3)(x + 1) made with one x × x square, strips of x × 1 and unit squares. Count the pieces.", "tag": "", "marks": "", "flat": [{"t": "Number of x × 1 strips = __B1__", "a": {"B1": "4"}}, {"t": "Number of unit squares = __B1__", "a": {"B1": "3"}}, {"t": "(x + 3)(x + 1) = x<sup>2</sup> + __B1__x + __B2__", "a": {"B1": "4", "B2": "3"}}], "sol": "3 strips on the right and 1 strip below: 4 strips (3x + x = 4x).\nThe corner gap is 3 units long and 1 unit wide: 3 unit squares.\nArea of the square + 4 strips + 3 unit squares = x<sup>2</sup> + 4x + 3.", "fig": "<svg class=\"figsvg\" viewBox=\"0 0 218 154\" role=\"img\"><defs><marker id=\"ah\" viewBox=\"0 0 10 10\" refX=\"9\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M0,1 L10,5 L0,9 z\" class=\"ahd\"/></marker><marker id=\"ahs\" viewBox=\"0 0 10 10\" refX=\"1\" refY=\"5\" markerWidth=\"7\" markerHeight=\"7\" orient=\"auto\"><path d=\"M10,1 L0,5 L10,9 z\" class=\"ahd\"/></marker></defs><rect class=\"sh\" x=\"34\" y=\"26\" width=\"96\" height=\"96\"/><rect class=\"sh3\" x=\"130\" y=\"26\" width=\"18\" height=\"96\"/><rect class=\"sh3\" x=\"148\" y=\"26\" width=\"18\" height=\"96\"/><rect class=\"sh3\" x=\"166\" y=\"26\" width=\"18\" height=\"96\"/><rect class=\"sh3\" x=\"34\" y=\"122\" width=\"96\" height=\"18\"/><rect class=\"cell\" x=\"130\" y=\"122\" width=\"18\" height=\"18\"/><rect class=\"cell\" x=\"148\" y=\"122\" width=\"18\" height=\"18\"/><rect class=\"cell\" x=\"166\" y=\"122\" width=\"18\" height=\"18\"/><text class=\"lb\" x=\"82.0\" y=\"14.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">x</text><text class=\"lb\" x=\"157.0\" y=\"14.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">3</text><text class=\"lb\" x=\"20.0\" y=\"74.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">x</text><text class=\"lb\" x=\"20.0\" y=\"131.0\" text-anchor=\"middle\" dominant-baseline=\"middle\">1</text><line class=\"tick\" x1=\"34.0\" y1=\"22.0\" x2=\"130.0\" y2=\"22.0\"/><line class=\"tick\" x1=\"130.0\" y1=\"22.0\" x2=\"184.0\" y2=\"22.0\"/><line class=\"tick\" x1=\"30.0\" y1=\"26.0\" x2=\"30.0\" y2=\"122.0\"/><line class=\"tick\" x1=\"30.0\" y1=\"122.0\" x2=\"30.0\" y2=\"140.0\"/></svg>"}, {"kind": "mcq", "text": "<b>Maths Lab Activity</b> · Which set of tiles makes a rectangle showing (x + 2)(x + 4)?", "opts": ["1 square, 6 strips and 8 unit squares", "1 square, 8 strips and 6 unit squares", "1 square, 6 strips and 6 unit squares", "2 squares, 6 strips and 8 unit squares"], "correct": 0, "tag": "", "sol": "(x + 2)(x + 4) = x<sup>2</sup> + 2x + 4x + 8 = x<sup>2</sup> + 6x + 8: one x × x square, 2 + 4 = 6 strips and 2 × 4 = 8 unit squares."}, {"kind": "blank", "p": "A rectangular vegetable garden has an area of (x<sup>2</sup> + 9x + 20) m<sup>2</sup>. Find its length and breadth (the length is the longer side; type it first).", "tag": "", "marks": "", "flat": [{"t": "Length × breadth = x<sup>2</sup> + 9x + 20 = (__B1__)(__B2__)", "a": {"B1": "x+5", "B2": "x+4"}, "expr": true}], "sol": "Product 20, sum 9: 4 and 5. x<sup>2</sup> + 9x + 20 = (x + 5)(x + 4): length (x + 5) m, breadth (x + 4) m."}]}];
/* ================= build slide lists ================= */
var SLIDES = {}; SECTIONS.forEach(function(sec){ SLIDES[sec.id]=sec.slides; });
var TAB_DEFS = SECTIONS.map(function(sec){ return {id:sec.id, label:sec.label, sub:sec.sub}; });

/* ================= state ================= */
function blankItem(slide){
  if(slide.kind==='mcq'||slide.kind==='ar'){
    return {status:'unanswered', attempts:0, choice:undefined};
  }
  return {status:'unanswered', curStep:0, stepStates: slide.flat.map(function(){ return {status:'unanswered', attempts:0, inputs:{}}; })};
}
function defaultState(){
  var s={tabs:{}};
  TAB_DEFS.forEach(function(td){
    s.tabs[td.id] = { idx:0, maxReached:0, items: SLIDES[td.id].map(function(sl){ return blankItem(sl); }) };
  });
  return s;
}
var appState = {mode:null, learning:null, quiz:null};
var state = null;      // alias for appState[appState.mode]
var MODE = null;
var activeTab=TAB_DEFS[0].id;

function setMode(m){
  appState.mode = m;
  if(!appState[m]) appState[m] = defaultState();
  state = appState[m];
  MODE = m;
}
/* ================= student accounts (saved on this device) ================= */
var SHEET_KEY='sopaan-c8-ch8';
var ACC_KEY='sopaan-students-v1';
var storageOK=true;
var MEM={};
function lsGet(k){ var r=null; try{ r=localStorage.getItem(k); }catch(e){ storageOK=false; }
  if(r===null||r===undefined) r=MEM[k]||null; try{ return r?JSON.parse(r):null; }catch(e){ return null; } }
function lsSet(k,v){ var j=JSON.stringify(v); MEM[k]=j; try{ localStorage.setItem(k,j); return true; }catch(e){ storageOK=false; return false; } }
try{ localStorage.setItem('sopaan-probe','1'); localStorage.removeItem('sopaan-probe'); }catch(e){ storageOK=false; }
function hashPin(pin,salt){ var h=2166136261, str=salt+'|'+pin; for(var i=0;i<str.length;i++){ h^=str.charCodeAt(i); h=Math.imul(h,16777619)>>>0; } return h.toString(36); }
function accKey(name,roll){ return (name.trim().toLowerCase().replace(/\s+/g,' ')+'#'+roll.trim().toLowerCase()); }
function accounts(){ return lsGet(ACC_KEY)||{students:{}}; }
var student=null; // {key,name,roll}
var records=[];   // finished-tab history for this student on this sheet
function progKey(){ return SHEET_KEY+'::'+student.key; }

function loadState(){
  appState = {mode:null, learning:null, quiz:null}; records=[]; activeTab=TAB_DEFS[0].id;
  var saved=lsGet(progKey());
  if(!saved) return;
  try{
    ['learning','quiz'].forEach(function(m){
      if(saved[m] && saved[m].tabs){
        var fresh = defaultState();
        TAB_DEFS.forEach(function(td){
          if(saved[m].tabs[td.id] && saved[m].tabs[td.id].items && saved[m].tabs[td.id].items.length===SLIDES[td.id].length){
            fresh.tabs[td.id] = saved[m].tabs[td.id];
          }
        });
        appState[m] = fresh;
      }
    });
    if(saved.mode==='learning' || saved.mode==='quiz') appState.mode = saved.mode;
    if(saved.activeTab && TAB_DEFS.some(function(t){ return t.id===saved.activeTab; })) activeTab = saved.activeTab;
    if(Array.isArray(saved.records)) records = saved.records;
  }catch(e){}
}
function saveState(){
  if(!student) return;
  lsSet(progKey(), {mode:appState.mode, learning:appState.learning, quiz:appState.quiz, activeTab:activeTab, records:records, updated:Date.now()});
  var acc=accounts(); if(acc.students[student.key]){ acc.students[student.key].last=Date.now(); lsSet(ACC_KEY,acc); }
}
function addRecord(mode, tabId, correct, revealed, skipped, total){
  records.unshift({at:Date.now(), mode:mode, tab:tabId, correct:correct, revealed:revealed, skipped:skipped, total:total});
  if(records.length>60) records.length=60;
  saveState();
}
function fmtDate(ms){ try{ return new Date(ms).toLocaleString(undefined,{day:'numeric',month:'short',hour:'2-digit',minute:'2-digit'}); }catch(e){ return ''; } }

function renderWho(){
  var el=document.getElementById('whoBar');
  if(!student){ el.hidden=true; el.innerHTML=''; return; }
  el.hidden=false;
  el.innerHTML='Signed in as <b>'+esc(student.name)+'</b> · Roll '+esc(student.roll)+
    ' <button id="btnRecord">My record</button> <button id="btnSignOut">Sign out</button>';
  document.getElementById('btnRecord').addEventListener('click', renderRecord);
  document.getElementById('btnSignOut').addEventListener('click', signOut);
}
function signOut(){
  saveState(); student=null; state=null; MODE=null;
  var acc=accounts(); delete acc.current; lsSet(ACC_KEY,acc);
  document.getElementById('modePill').hidden=true;
  renderWho(); renderLogin();
}
function signIn(key){
  var acc=accounts(); var st=acc.students[key]; if(!st) return;
  student={key:key, name:st.name, roll:st.roll};
  acc.current=key; st.last=Date.now(); lsSet(ACC_KEY,acc);
  loadState(); renderWho();
  if(appState.mode==='learning' || appState.mode==='quiz'){
    setMode(appState.mode); document.getElementById('modePill').hidden=false; updateModePill();
    showChrome(true); buildTabbar(); renderSlide();
    showToast('Welcome back, '+st.name.split(' ')[0]+'. Picking up where you left off.');
  } else {
    renderModePicker();
  }
}
function renderLogin(msg){
  showChrome(false);
  var acc=accounts(); var keys=Object.keys(acc.students).sort(function(a,b){ return (acc.students[b].last||0)-(acc.students[a].last||0); });
  var h='<div class="login-card"><h2>Student sign-in</h2>'+
    '<p class="lead">Sign in to save your answers, see your record and continue where you left off next time.</p>';
  if(!storageOK) h+='<div class="login-err">This browser is blocking saved data (for example, a private window). You can still practise, but progress will not be kept after you close the page.</div>';
  if(msg) h+='<div class="login-err">'+esc(msg)+'</div>';
  if(keys.length){
    h+='<div class="known"><span class="known-label">Continue as</span>';
    keys.slice(0,6).forEach(function(k){ var st=acc.students[k];
      h+='<button class="known-btn" data-k="'+esc(k)+'"><span>'+esc(st.name)+' <small>· Roll '+esc(st.roll)+'</small></span><small>'+(st.last?'Last active '+fmtDate(st.last):'')+'</small></button>'; });
    h+='</div>';
  }
  h+='<form id="loginForm" novalidate>'+
    '<div class="fld"><label for="lgName">Full name</label><input id="lgName" autocomplete="name" required></div>'+
    '<div class="fld-row"><div class="fld"><label for="lgRoll">Roll no.</label><input id="lgRoll" required></div>'+
    '<div class="fld"><label for="lgPin">4-digit PIN</label><input id="lgPin" type="password" inputmode="numeric" maxlength="4" autocomplete="off" required></div></div>'+
    '<button class="btn btn-primary" type="submit" style="width:100%;">Sign in</button>'+
    '<p class="login-note">New here? Enter your name, roll no. and a PIN you will remember, and your profile is created. Progress is saved on this device, so use the same device and browser to continue.</p>'+
    '</form></div>';
  var wrap=document.getElementById('wrap'); wrap.innerHTML=h;
  wrap.querySelectorAll('.known-btn').forEach(function(b){
    b.addEventListener('click', function(){
      var st=accounts().students[b.dataset.k];
      document.getElementById('lgName').value=st.name; document.getElementById('lgRoll').value=st.roll;
      document.getElementById('lgPin').focus();
    });
  });
  document.getElementById('loginForm').addEventListener('submit', function(e){
    e.preventDefault();
    var name=document.getElementById('lgName').value.trim(), roll=document.getElementById('lgRoll').value.trim(), pin=document.getElementById('lgPin').value.trim();
    if(!name||!roll){ renderLoginErr('Enter your name and roll no.'); return; }
    if(!/^\d{4}$/.test(pin)){ renderLoginErr('Your PIN must be exactly 4 digits.'); return; }
    var k=accKey(name,roll); var acc=accounts();
    if(acc.students[k]){
      if(acc.students[k].pin!==hashPin(pin,k)){ renderLoginErr('That PIN does not match this name and roll no. Try again.'); return; }
    } else {
      acc.students[k]={name:name.replace(/\s+/g,' '), roll:roll, pin:hashPin(pin,k), created:Date.now()};
      lsSet(ACC_KEY,acc);
    }
    signIn(k);
  });
}
function renderLoginErr(m){
  var n=document.getElementById('lgName').value, r=document.getElementById('lgRoll').value;
  renderLogin(m); document.getElementById('lgName').value=n; document.getElementById('lgRoll').value=r; document.getElementById('lgPin').focus();
}
function tabSummary(m){
  var st=appState[m]; if(!st) return null;
  return TAB_DEFS.map(function(td){
    var items=st.tabs[td.id].items, n=items.length;
    var c=items.filter(function(i){return i.status==='correct';}).length;
    var done=items.filter(function(i){return i.status!=='unanswered';}).length;
    return {label:td.label, n:n, done:done, correct:c};
  });
}
function renderRecord(){
  showChrome(false);
  var tabLabel=function(id){ var t=TAB_DEFS.find(function(x){return x.id===id;}); return t?t.label:id; };
  var h='<div class="rec-card"><h2>'+esc(student.name)+'’s record</h2><p class="rec-empty" style="margin:0 0 12px;">Roll '+esc(student.roll)+' · Factorisation</p>';
  ['learning','quiz'].forEach(function(m){
    var sum=tabSummary(m);
    h+='<h3>'+(m==='learning'?'📘 Learning Sheet':'📝 Quiz Mode')+'</h3>';
    if(!sum){ h+='<p class="rec-empty">Not started yet.</p>'; return; }
    h+='<div class="rec-table-wrap"><table class="rec-table"><thead><tr><th>Section</th><th>Progress</th><th>Correct first time</th></tr></thead><tbody>';
    sum.forEach(function(r){ var pct=Math.round(r.done/r.n*100);
      h+='<tr><td>'+r.label+'</td><td><span class="rec-bar"><i style="width:'+pct+'%"></i></span>'+r.done+' / '+r.n+'</td><td>'+(m==='quiz'?'—':r.correct+' / '+r.n)+'</td></tr>'; });
    h+='</tbody></table></div>';
  });
  h+='</div><div class="rec-card"><h3>Finished sections</h3>';
  if(!records.length){ h+='<p class="rec-empty">No sections finished yet. Each time you finish a tab, your score is recorded here.</p>'; }
  else {
    h+='<div class="rec-table-wrap"><table class="rec-table"><thead><tr><th>When</th><th>Mode</th><th>Section</th><th>Score</th></tr></thead><tbody>';
    records.forEach(function(r){ h+='<tr><td>'+fmtDate(r.at)+'</td><td>'+(r.mode==='quiz'?'Quiz':'Learning')+'</td><td>'+tabLabel(r.tab)+'</td><td>'+r.correct+' / '+r.total+(r.mode==='learning'?' <span class="rec-empty">('+r.revealed+' revealed, '+r.skipped+' skipped)</span>':'')+'</td></tr>'; });
    h+='</tbody></table></div>';
  }
  h+='</div><button class="btn btn-primary" id="btnBack">'+(MODE?'Back to practice':'Choose a mode')+'</button>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('btnBack').addEventListener('click', function(){
    if(MODE){ showChrome(true); buildTabbar(); renderSlide(); } else renderModePicker();
  });
  window.scrollTo({top:0});
}

/* ================= tab bar ================= */
function buildTabbar(){
  var el=document.getElementById('tabbar');
  el.innerHTML = '<button class="tab-btn'+(activeTab==='theory'?' active':'')+'" data-tab="theory">Theory<span class="tab-count">notes</span></button>' + TAB_DEFS.map(function(t){
    return '<button class="tab-btn'+(t.id===activeTab?' active':'')+'" data-tab="'+t.id+'">'+t.label+'<span class="tab-count">'+SLIDES[t.id].length+'</span></button>';
  }).join('') + '<button class="tab-btn'+(activeTab==='report'?' active':'')+'" data-tab="report">📊 Report<span class="tab-count">pie</span></button>';
  el.querySelectorAll('.tab-btn').forEach(function(btn){
    btn.addEventListener('click', function(){ activeTab=btn.dataset.tab; buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); });
  });
}

/* ================= rendering ================= */
function canProceed(item){ return item.status==='correct'||item.status==='revealed'||item.status==='skipped'; }

function renderSlide(){
  if(!state.tabs[activeTab]){ activeTab=TAB_DEFS[0].id; }
  var ts = state.tabs[activeTab];
  var slides = SLIDES[activeTab];
  if(ts.idx>=slides.length||ts.idx<0) ts.idx=0;
  var idx = ts.idx;
  var slide = slides[idx];
  var item = ts.items[idx];

  var tdef=TAB_DEFS.find(function(t){return t.id===activeTab;});
  document.getElementById('spLabel').textContent = tdef.label+' · Question '+(idx+1)+' of '+slides.length;
  document.getElementById('secSub').textContent = tdef.label+' — '+tdef.sub;
  document.getElementById('spFill').style.width = Math.round(((idx)/(Math.max(slides.length-1,1)))*100)+'%';

  var h='<div class="qcard">';
  if(slide.kind==='mcq' || slide.kind==='ar'){
    h += renderChoiceBody(slide, item, idx);
  } else {
    h += renderBlankBody(slide, item, idx);
  }
  h += '<div class="feedback" id="feedbackBox"></div>';
  if(MODE==='learning' && (item.status==='correct'||item.status==='revealed')) h += solutionHTML(slide);
  h += '</div>';

  document.getElementById('wrap').innerHTML = h;
  wireSlideEvents(slide, item, idx);
  restoreFeedback(item);
  renderNavbar(item);
  updateFab();
  saveState();
}

function solutionHTML(slide){
  if(!slide.sol) return '';
  return '<div class="solution"><div class="sol-h">Solution</div>'+slide.sol.split('\n').map(function(l){ return '<div class="sol-line">'+fr(esc(l))+'</div>'; }).join('')+'</div>';
}
var LETTERS=['a','b','c','d','e','f'];
function chosenHTML(slide,item){
  var opts = slide.kind==='mcq' ? slide.opts : AR_OPTIONS;
  if(item.choice===undefined) return '';
  var ok=item.choice===slide.correct;
  var h='<div class="chosen '+(ok?'ok':'bad')+'"><b>Your answer:</b> ('+LETTERS[item.choice]+') '+fr(esc(opts[item.choice]))+(ok?' ✓':' ✗')+'</div>';
  if(!ok) h+='<div class="chosen ok"><b>Correct answer:</b> ('+LETTERS[slide.correct]+') '+fr(esc(opts[slide.correct]))+'</div>';
  return h;
}
function renderChoiceBody(slide, item, idx){
  var h='<div class="qhead"><div class="qnum">'+(idx+1)+'</div>';
  if(slide.kind==='mcq'){
    h += '<div class="qtext">'+fr(esc(slide.text))+(slide.tag?'<span class="qtag">'+esc(slide.tag)+'</span>':'')+'</div>';
  } else {
    h += '<div class="qtext"><span class="qtag" style="margin-left:0;margin-right:6px;">A / R</span>Assertion (A): '+fr(esc(slide.a))+'<br>Reason (R): '+fr(esc(slide.r))+(slide.tag?'<span class="qtag">'+esc(slide.tag)+'</span>':'')+'</div>';
  }
  h += '</div>';
  if(slide.fig) h += '<div class="fig">'+slide.fig+'</div>';
  var opts = slide.kind==='mcq' ? slide.opts : AR_OPTIONS;
  if(MODE==='quiz') h += '<div class="quiz-hint">Quiz mode — pick an option. The correct answer is revealed once you finish this tab.</div>';
  var locked = MODE==='learning' && (item.status==='correct' || item.status==='revealed');
  h += '<div class="options">';
  opts.forEach(function(opt,i){
    var cls='opt';
    if(locked){
      if(i===slide.correct) cls+=' is-correct';
      else if(item.choice===i) cls+=' is-wrong';
      cls+=' locked';
    }
    if(item.choice===i) cls+=' is-chosen';
    h += '<label class="'+cls+'"><input type="radio" name="choice" value="'+i+'" '+(item.choice===i?'checked':'')+' '+(locked?'disabled':'')+'> <span class="opt-l">('+LETTERS[i]+')</span> <span class="opt-t">'+fr(esc(opt))+'</span>'+(item.choice===i?'<span class="opt-tag">'+(locked?(i===slide.correct?'Your answer ✓':'Your answer ✗'):'Selected')+'</span>':'')+'</label>';
  });
  h += '</div>';
  if(locked) h += chosenHTML(slide,item);
  return h;
}

function renderBlankBody(slide, item, idx){
  var h='<div class="qhead"><div class="qnum">'+(idx+1)+'</div><div class="qtext">';
  if(slide.kind==='case'){ h += '<b>'+esc(slide.title)+'.</b> '+esc(slide.body); }
  else { var pp=fr(esc(slide.p)).split('\n'); h += pp[0]+pp.slice(1).map(function(x){return '<span class="datline">'+x+'</span>';}).join(''); }
  h += (slide.tag?'<span class="qtag">'+esc(slide.tag)+'</span>':'')+'</div>';
  if(slide.marks) h += '<div class="marks-pill">'+slide.marks+'</div>';
  h += '</div>';
  if(slide.fig) h += '<div class="fig">'+slide.fig+'</div>';

  var total = slide.flat.length;
  var hasExpr = slide.flat.some(function(s){ return s.expr===true; });
  var hasFrac = slide.flat.some(function(s){ return ['fv','fe','fl','fm','fi','flist'].indexOf(s.expr)>=0; });
  var blankCount=0; slide.flat.forEach(function(s){ blankCount+=Object.keys(s.a).length; });

  if(MODE==='quiz'){
    h += '<div class="step-badge">'+blankCount+' blank'+(blankCount===1?'':'s')+' in this question</div>';
    h += '<div class="quiz-hint">Quiz mode — fill in what you can. Correct answers are revealed once you finish this tab.</div>';
    if(hasFrac) h += '<div class="quiz-hint">Type fractions like <b>3/4</b> and mixed numbers like <b>2 3/4</b> (whole number, space, fraction).</div>';
    if(hasExpr) h += '<div class="quiz-hint">Type answers like <b>2x-5</b>, <b>x^2-4</b> or <b>3+2i</b>. Use ^ for powers, | | for absolute value and pi for π. Any equivalent form is accepted.</div>';
    h += '<div class="steps">';
    for(var qi=0; qi<total; qi++){
      var qstep = slide.flat[qi];
      var qss = item.stepStates[qi];
      if(qstep.partLabel) h += '<div class="subpart-label">'+esc(qstep.partLabel)+'</div>';
      if(qstep.orDivider) h += '<div class="or-divider">OR</div>';
      var qline = fr(qstep.t);
      Object.keys(qstep.a).forEach(function(bkey){
        var val = qss.inputs[bkey]||'';
        var inp='<input class="blank-input'+(['fv','fe','fl','fm','fi','dec'].indexOf(qstep.expr)>=0?' fr':(qstep.expr==='flist'||qstep.expr==='dlist'||qstep.expr==='glist')?' wide':qstep.expr===true?' expr':(qstep.expr==='words'||qstep.expr==='list'||qstep.expr==='expanded'||qstep.expr==='set'||qstep.expr==='primes'||qstep.expr==='trans'||qstep.expr==='rot'?' wide':''))+'" data-step="'+qi+'" data-bkey="'+bkey+'" value="'+esc(val)+'">';
        qline = qline.replace('__'+bkey+'__', inp);
      });
      if(qstep.fig) h += '<div class="fig">'+qstep.fig+'</div>';
      h += '<div class="step-line">'+qline+'</div>';
    }
    h += '</div>';
    return h;
  }

  if(hasFrac) h += '<div class="quiz-hint">Type fractions like <b>3/4</b> and mixed numbers like <b>2 3/4</b> (whole number, space, fraction).</div>';
  if(hasExpr) h += '<div class="quiz-hint">Type answers like <b>2x-5</b>, <b>x^2-4</b> or <b>3+2i</b>. Use ^ for powers, | | for absolute value and pi for π. Any equivalent form is accepted.</div>';
  var locked = item.status==='correct' || item.status==='revealed';
  var upto = locked ? total-1 : item.curStep;
  h += '<div class="step-badge">Step '+(Math.min(item.curStep,total-1)+1)+' of '+total+'</div>';
  h += '<div class="steps">';
  for(var i=0;i<=upto;i++){
    var step = slide.flat[i];
    var ss = item.stepStates[i];
    if(step.partLabel) h += '<div class="subpart-label">'+esc(step.partLabel)+'</div>';
    if(step.orDivider) h += '<div class="or-divider">OR</div>';
    var resolved = ss.status==='correct' || ss.status==='revealed';
    var line = fr(step.t);
    Object.keys(step.a).forEach(function(bkey){
      var fs = ss.inputs['fs_'+bkey];
      var cls = fs==='correct'?'is-correct':(fs==='wrong'?'is-wrong':'');
      var val = ss.inputs[bkey]||'';
      var dis = resolved ? 'disabled':'';
      var inp='<input class="blank-input '+cls+(['fv','fe','fl','fm','fi','dec'].indexOf(step.expr)>=0?' fr':(step.expr==='flist'||step.expr==='dlist'||step.expr==='glist')?' wide':step.expr===true?' expr':(step.expr==='words'||step.expr==='list'||step.expr==='expanded'||step.expr==='set'||step.expr==='primes'||step.expr==='trans'||step.expr==='rot'?' wide':''))+'" data-step="'+i+'" data-bkey="'+bkey+'" value="'+esc(val)+'" '+dis+'>';
      line = line.replace('__'+bkey+'__', inp);
    });
    if(step.fig) h += '<div class="fig">'+step.fig+'</div>';
    h += '<div class="step-line'+(resolved?' resolved':'')+'">'+line+'</div>';
    if(ss.status==='revealed'){
      var reveals=[];
      Object.keys(step.a).forEach(function(bkey){ reveals.push(bkey+' = '+esc(step.a[bkey])); });
      h += '<div class="reveal-note">correct: '+reveals.join(', ')+'</div>';
    }
  }
  h += '</div>';
  return h;
}

var __flash=null;
function restoreFeedback(item){
  var fb=document.getElementById('feedbackBox'); if(!fb) return;
  if(__flash){ fb.className='feedback show '+__flash.c; fb.innerHTML=__flash.h; __flash=null; return; }
  if(MODE==='quiz'){ fb.className='feedback'; fb.innerHTML=''; return; }
  if(item.status==='correct'){ fb.className='feedback show ok'; fb.innerHTML='<b>All done — correct!</b>'; }
  else if(item.status==='revealed'){ fb.className='feedback show reveal'; fb.innerHTML='<b>Answer revealed above.</b> Move on whenever you’re ready.'; }
  else if(item.status==='skipped'){ fb.className='feedback show retry'; fb.innerHTML='Skipped — you can answer it now and press <b>Check</b>, or come back later from the Question Palette.'; }
  else { fb.className='feedback'; fb.innerHTML=''; }
}

function slideIsAnswered(slide,item){
  if(slide.kind==='mcq'||slide.kind==='ar') return item.choice!==undefined;
  return item.stepStates.some(function(ss){ return Object.keys(ss.inputs).some(function(k){ return k.indexOf('fs_')!==0 && ss.inputs[k] && String(ss.inputs[k]).trim()!==''; }); });
}
function renderNavbar(item){
  var ts = state.tabs[activeTab];
  var slides = SLIDES[activeTab];
  var slide = slides[ts.idx];
  var isLast = ts.idx === slides.length-1;
  var h='';
  h += '<button class="btn" id="btnPrev" '+(ts.idx===0?'disabled':'')+'>&larr; Previous</button>';

  if(MODE==='quiz'){
    h += '<button class="btn" id="btnSkip">Skip</button>';
    h += '<span class="spacer"></span>';
    h += '<button class="btn btn-primary" id="btnNext">'+(isLast?'Finish':'Next →')+'</button>';
  } else {
    h += '<button class="btn" id="btnSkip" '+(item.status!=='unanswered'?'disabled':'')+'>Skip</button>';
    h += '<span class="spacer"></span>';
    h += '<button class="btn btn-primary" id="btnCheck" '+((item.status!=='unanswered'&&item.status!=='skipped')?'disabled':'')+'>Check</button>';
    h += '<button class="btn btn-primary" id="btnNext" '+(canProceed(item)?'':'disabled')+'>'+(isLast?'Finish':'Next →')+'</button>';
  }
  document.getElementById('navbarInner').innerHTML = h;

  document.getElementById('btnPrev').addEventListener('click', function(){ if(ts.idx>0){ ts.idx--; renderSlide(); } });

  if(MODE==='quiz'){
    document.getElementById('btnSkip').addEventListener('click', function(){
      item.status='skipped';
      advanceQuiz(ts, slides);
    });
    document.getElementById('btnNext').addEventListener('click', function(){
      item.status = slideIsAnswered(slide,item) ? 'answered' : 'unanswered';
      advanceQuiz(ts, slides);
    });
  } else {
    document.getElementById('btnSkip').addEventListener('click', function(){
      if(item.status==='unanswered'){ item.status='skipped'; renderSlide(); }
    });
    document.getElementById('btnNext').addEventListener('click', function(){
      if(!canProceed(item)) return;
      if(ts.idx < slides.length-1){ ts.idx++; ts.maxReached=Math.max(ts.maxReached, ts.idx); renderSlide(); }
      else { renderFinished(); }
    });
    document.getElementById('btnCheck').addEventListener('click', function(){ handleCheck(); });
  }
}
function advanceQuiz(ts, slides){
  if(ts.idx < slides.length-1){ ts.idx++; ts.maxReached=Math.max(ts.maxReached, ts.idx); renderSlide(); }
  else { renderFinished(); }
}

function wireSlideEvents(slide, item, idx){
  document.querySelectorAll('input[type="radio"][name="choice"]').forEach(function(r){
    r.addEventListener('change', function(){ item.choice=parseInt(r.value,10); saveState();
      document.querySelectorAll('.opt').forEach(function(l){ l.classList.remove('is-chosen'); var t=l.querySelector('.opt-tag'); if(t) t.remove(); });
      var lab=r.closest('.opt'); lab.classList.add('is-chosen'); var tg=document.createElement('span'); tg.className='opt-tag'; tg.textContent='Selected'; lab.appendChild(tg); });
  });
  document.querySelectorAll('.blank-input').forEach(function(inp){
    inp.addEventListener('input', function(){
      var si=parseInt(inp.dataset.step,10), bk=inp.dataset.bkey;
      item.stepStates[si].inputs[bk]=inp.value;
      saveState();
    });
  });
}

function handleCheck(){
  var ts = state.tabs[activeTab];
  var slide = SLIDES[activeTab][ts.idx];
  var item = ts.items[ts.idx];
  if(item.status==='skipped') item.status='unanswered';
  var fb = document.getElementById('feedbackBox');

  if(slide.kind==='mcq' || slide.kind==='ar'){
    if(item.choice===undefined){ fb.className='feedback show err'; fb.innerHTML='Please choose an option first.'; return; }
    var ok = item.choice===slide.correct;
    if(ok){
      item.status='correct'; playSuccess();
      fb.className='feedback show ok'; fb.innerHTML='<b>Correct!</b>';
    } else {
      item.attempts=(item.attempts||0)+1;
      if(item.attempts>=2){ item.status='revealed'; playReveal(); }
      else { playWrong(); fb.className='feedback show retry'; fb.innerHTML='<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'; __flash={c:'retry',h:'<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'}; }
    }
    renderSlide();
    return;
  }

  // blank / case
  var step = slide.flat[item.curStep];
  var ss = item.stepStates[item.curStep];
  var blanks = Object.keys(step.a);
  var missing = blanks.some(function(k){ return !ss.inputs[k] || String(ss.inputs[k]).trim()===''; });
  if(missing){ fb.className='feedback show err'; fb.innerHTML='Fill in every blank in this step before checking.'; return; }

  var allGood=true;
  blanks.forEach(function(k){
    var good=answerMatches(ss.inputs[k], step.a[k], step.accept, step.expr);
    ss.inputs['fs_'+k]=good?'correct':'wrong';
    if(!good) allGood=false;
  });

  if(allGood){
    ss.status='correct'; playSuccess();
    advanceStepOrFinish(item, slide);
  } else {
    ss.attempts=(ss.attempts||0)+1;
    if(ss.attempts>=2){
      ss.status='revealed'; playReveal();
      advanceStepOrFinish(item, slide);
    } else {
      playWrong();
      fb.className='feedback show retry';
      fb.innerHTML='<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'; __flash={c:'retry',h:'<b>Not quite — try again</b> (attempt 1 of 2) <span class="attempts-dots"><span class="used"></span><span></span></span>'};
      renderSlide();
      return;
    }
  }
  renderSlide();
}

function advanceStepOrFinish(item, slide){
  var total = slide.flat.length;
  var hasExpr = slide.flat.some(function(s){ return s.expr===true; });
  var hasFrac = slide.flat.some(function(s){ return ['fv','fe','fl','fm','fi','flist'].indexOf(s.expr)>=0; });
  if(item.curStep < total-1){
    item.curStep++;
  } else {
    var anyRevealed = item.stepStates.some(function(ss){ return ss.status==='revealed'; });
    item.status = anyRevealed ? 'revealed' : 'correct';
  }
}

/* ================= palette ================= */
var overlay=document.getElementById('overlay');
function statusOf(tabId,i){
  var ts=state.tabs[tabId];
  if(i>ts.maxReached) return 'locked';
  if(i===ts.idx) return 'current';
  return ts.items[i].status==='unanswered' ? 'locked' : ts.items[i].status;
}
function buildPalette(){
  var slides=SLIDES[activeTab];
  document.getElementById('paletteTitle').textContent = TAB_DEFS.find(function(t){return t.id===activeTab;}).label+' · Question palette';
  var body=document.getElementById('paletteBody');
  var html='';
  for(var i=0;i<slides.length;i++){
    html += '<div class="chip" data-status="'+statusOf(activeTab,i)+'" data-idx="'+i+'">'+(i+1)+'</div>';
  }
  body.innerHTML = html;
  body.querySelectorAll('.chip').forEach(function(chip){
    chip.addEventListener('click', function(){
      var i=parseInt(chip.dataset.idx,10);
      var ts=state.tabs[activeTab];
      if(i>ts.maxReached){ showToast('Finish the earlier questions to unlock this one.'); return; }
      ts.idx=i; closePalette(); renderSlide();
    });
  });
}
function openPalette(){ buildPalette(); overlay.classList.add('show'); }
function closePalette(){ overlay.classList.remove('show'); }
var toastTimer;
function showToast(msg){
  var t=document.getElementById('toast'); t.textContent=msg; t.classList.add('show');
  clearTimeout(toastTimer); toastTimer=setTimeout(function(){ t.classList.remove('show'); },2200);
}
document.getElementById('paletteFab').addEventListener('click', openPalette);
document.getElementById('paletteClose').addEventListener('click', closePalette);
overlay.addEventListener('click', function(e){ if(e.target===overlay) closePalette(); });

function updateFab(){
  var slides=SLIDES[activeTab];
  var done=state.tabs[activeTab].items.filter(function(it){ return it.status==='correct'||it.status==='revealed'||it.status==='skipped'; }).length;
  document.getElementById('fabBadge').textContent = done+'/'+slides.length;
}

/* ================= overall progress + finish ================= */
function updateChapterProgress(){
  var total=0, done=0;
  TAB_DEFS.forEach(function(td){
    state.tabs[td.id].items.forEach(function(it){
      total++;
      if(it.status==='correct'||it.status==='revealed'||it.status==='skipped') done++;
    });
  });
  var pct = total>0 ? Math.round((done/total)*100) : 0;
  document.getElementById('cpFill').style.width=pct+'%';
  document.getElementById('cpLabel').textContent=pct+'% complete';
}
function isSlideCorrect(tabId,i){
  var slide=SLIDES[tabId][i], item=state.tabs[tabId].items[i];
  if(slide.kind==='mcq'||slide.kind==='ar') return item.choice===slide.correct;
  return slide.flat.every(function(step,si){
    var ss=item.stepStates[si];
    return Object.keys(step.a).every(function(k){ return answerMatches(ss.inputs[k], step.a[k], step.accept, step.expr); });
  });
}
function isSlideAttempted(tabId,i){
  var slide=SLIDES[tabId][i], item=state.tabs[tabId].items[i];
  if(slide.kind==='mcq'||slide.kind==='ar') return item.choice!==undefined;
  return slide.flat.some(function(step,si){
    var ss=item.stepStates[si];
    return Object.keys(step.a).some(function(k){ return ss.inputs[k] && String(ss.inputs[k]).trim()!==''; });
  });
}
function slideLabel(slide){
  var t = slide.kind==='case' ? slide.title : (slide.kind==='ar' ? slide.a : (slide.p||slide.text||''));
  t = String(t);
  return t.length>90 ? t.slice(0,90)+'…' : t;
}
function slideAnswerText(slide, item, correctSide){
  if(slide.kind==='mcq'||slide.kind==='ar'){
    var opts = slide.kind==='mcq' ? slide.opts : AR_OPTIONS;
    if(correctSide) return '('+LETTERS[slide.correct]+') '+opts[slide.correct];
    return item.choice!==undefined ? '('+LETTERS[item.choice]+') '+opts[item.choice] : '— not attempted —';
  }
  return slide.flat.map(function(step,si){
    var ss=item.stepStates[si];
    return Object.keys(step.a).map(function(k){
      return correctSide ? step.a[k] : (ss.inputs[k] || '—');
    }).join(', ');
  }).join('  |  ');
}

function renderFinished(){
  if(MODE==='quiz'){ renderQuizResults(); return; }
  var ts=state.tabs[activeTab];
  var correct=ts.items.filter(function(i){return i.status==='correct';}).length;
  var revealed=ts.items.filter(function(i){return i.status==='revealed';}).length;
  var skipped=ts.items.filter(function(i){return i.status==='skipped';}).length;
  var h='<div class="qcard done-card"><div class="big">✨</div><h2>Tab complete</h2>';
  h+='<p style="color:var(--ink-soft);font-size:14px;">You worked through all '+ts.items.length+' questions in this tab.</p>';
  if(!ts.recorded){ ts.recorded=true; addRecord('learning', activeTab, correct, revealed, skipped, ts.items.length); }
  h+='<div class="score-pills">'+
     '<span class="score-pill" style="background:var(--success-soft);color:var(--success);">'+correct+' correct</span>'+
     '<span class="score-pill" style="background:var(--danger-soft);color:var(--danger);">'+revealed+' revealed</span>'+
     '<span class="score-pill" style="background:var(--gold-soft);color:var(--retry-text);">'+skipped+' skipped</span></div>'+
     '<button class="btn btn-primary" id="btnReview">Review from question 1</button></div>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('navbarInner').innerHTML='';
  document.getElementById('btnReview').addEventListener('click', function(){ ts.idx=0; ts.recorded=false; renderSlide(); });
  updateChapterProgress(); saveState();
}

function renderQuizResults(){
  var ts=state.tabs[activeTab];
  var slides=SLIDES[activeTab];
  var correctCount=0, attemptedCount=0;
  var rowsHtml = slides.map(function(slide,i){
    var item=ts.items[i];
    var ok=isSlideCorrect(activeTab,i);
    var att=isSlideAttempted(activeTab,i);
    if(ok) correctCount++;
    if(att) attemptedCount++;
    var tagCls = ok?'ok':(att?'bad':'na');
    var tagText = ok?'Correct':(att?'Incorrect':'Not attempted');
    var row='<div class="review-row">';
    row+='<span class="review-tag '+tagCls+'">Q'+(i+1)+' · '+tagText+'</span>';
    row+='<div class="review-q">'+fr(esc(slideLabel(slide)))+'</div>';
    row+='<div class="review-ans"><b>Your answer:</b> '+fr(esc(slideAnswerText(slide,item,false)))+'</div>';
    if(!ok) row+='<div class="review-ans" style="color:var(--success);"><b>Correct answer:</b> '+fr(esc(slideAnswerText(slide,item,true)))+'</div>';
    if(slide.sol) row+='<details class="rev-sol"><summary>Show solution</summary>'+solutionHTML(slide)+'</details>';
    row+='</div>';
    return row;
  }).join('');

  addRecord('quiz', activeTab, correctCount, 0, 0, slides.length);
  var h='<div class="qcard done-card" style="text-align:left;">';
  h+='<div style="text-align:center;"><div class="big">🏁</div><h2>Quiz complete</h2>';
  h+='<p style="color:var(--ink-soft);font-size:14px;">'+correctCount+' / '+slides.length+' correct · '+attemptedCount+' attempted</p></div>';
  h+='<div style="margin-top:16px;">'+rowsHtml+'</div>';
  h+='<div style="text-align:center;margin-top:6px;"><button class="btn btn-primary" id="btnReview">Review from question 1</button></div>';
  h+='</div>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('navbarInner').innerHTML='';
  document.getElementById('btnReview').addEventListener('click', function(){ ts.idx=0; renderSlide(); });
  playSuccess();
  updateChapterProgress(); saveState();
}

/* ================= mode picker ================= */
function updateModePill(){
  var btn=document.getElementById('modePill');
  if(!btn) return;
  btn.textContent = MODE==='quiz' ? '📝 Quiz Mode · switch' : '📘 Learning Sheet · switch';
}
function showChrome(show){
  document.querySelector('.tabbar').style.display = show?'':'none';
  document.querySelector('.slide-progress').style.display = show?'':'none';
  document.querySelector('.navbar').style.display = show?'':'none';
  document.getElementById('paletteFab').style.display = show?'':'none';
}
function renderModePicker(){
  showChrome(false);
  var wrap=document.getElementById('wrap');
  wrap.innerHTML =
    '<div class="mode-pick">'+
      '<div class="mode-card" data-pick="learning"><div class="mode-icon">📘</div><h3>Learning Sheet</h3>'+
      '<p>Work through each question step by step with instant feedback — two tries, then the correct value is revealed right there so you can keep moving.</p></div>'+
      '<div class="mode-card" data-pick="quiz"><div class="mode-icon">📝</div><h3>Quiz Mode</h3>'+
      '<p>Attempt every question with no hints along the way. Your score and the full answer key are shown together once you finish the tab — just like the real exam.</p></div>'+
    '</div>';
  wrap.querySelectorAll('[data-pick]').forEach(function(card){
    card.addEventListener('click', function(){
      setMode(card.dataset.pick); activeTab='theory';
      document.getElementById('modePill').hidden=false;
      updateModePill();
      showChrome(true);
      buildTabbar();
      renderSlide();
    });
  });
}
document.getElementById('modePill').addEventListener('click', function(){
  if(!appState.mode){ return; }
  setMode(appState.mode==='quiz' ? 'learning' : 'quiz');
  updateModePill();
  showChrome(true);
  buildTabbar();
  renderSlide();
});

/* ================= boot ================= */
var _origRenderSlide = renderSlide;
renderSlide = function(){ _origRenderSlide(); updateChapterProgress(); updateModePill(); };


/* ================= theory tab ================= */
function renderTheory(){
  var wrap=document.getElementById('wrap');
  wrap.innerHTML='<div class="theory">'+fr(THEORY)+'</div>';
  document.querySelector('.slide-progress').style.display='none';
  document.querySelector('.navbar').style.display='none';
  document.getElementById('paletteFab').style.display='none';
  document.getElementById('secSub').textContent='Theory — notes, key ideas and worked examples';
  wrap.querySelectorAll('[data-go]').forEach(function(b){ b.addEventListener('click',function(){ activeTab=b.dataset.go; buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); }); });
  wrap.querySelectorAll('[data-jump]').forEach(function(b){ b.addEventListener('click',function(){ var t=document.getElementById(b.dataset.jump); if(t) t.scrollIntoView({behavior:'smooth',block:'start'}); }); });
}
var _rsTheory = renderSlide;
renderSlide = function(){
  if(activeTab==='theory'){ renderTheory(); buildTabbar(); updateChapterProgress(); updateModePill(); saveState(); return; }
  document.querySelector('.slide-progress').style.display='';
  document.querySelector('.navbar').style.display='';
  document.getElementById('paletteFab').style.display='';
  _rsTheory();
};


/* ================= MYP criterion report ================= */
var CRIT_COL={A:'var(--accent-text)',B:'var(--gold)',C:'#8A6FC4',D:'#2F9AA0'};
function critStats(){
  return REPORT.map(function(r){
    var tabId=r[0], slides=SLIDES[tabId], ts=state&&state.tabs[tabId];
    var c=0,w=0,n=0;
    slides.forEach(function(sl,i){
      if(!ts||!ts.items[i]){ n++; return; }
      var it=ts.items[i];
      if(MODE==='learning'){
        if(it.status==='correct') c++; else if(it.status==='revealed') w++; else n++;
      } else {
        if(isSlideCorrect(tabId,i)) c++; else if(isSlideAttempted(tabId,i)) w++; else n++;
      }
    });
    var tot=slides.length, pct=tot?Math.round(c/tot*100):0;
    var lvl=Math.round(c/Math.max(tot,1)*8);
    return {tab:tabId,letter:r[1],name:r[2],c:c,w:w,n:n,tot:tot,pct:pct,lvl:lvl};
  });
}
function arcPath(cx,cy,r,a0,a1){
  if(a1-a0>=Math.PI*2-1e-6){ return 'M'+(cx-r)+','+cy+' a'+r+','+r+' 0 1,0 '+(2*r)+',0 a'+r+','+r+' 0 1,0 '+(-2*r)+',0 Z'; }
  var x0=cx+r*Math.sin(a0), y0=cy-r*Math.cos(a0), x1=cx+r*Math.sin(a1), y1=cy-r*Math.cos(a1);
  return 'M'+cx+','+cy+' L'+x0.toFixed(2)+','+y0.toFixed(2)+' A'+r+','+r+' 0 '+((a1-a0)>Math.PI?1:0)+',1 '+x1.toFixed(2)+','+y1.toFixed(2)+' Z';
}
function pieSVG(parts,size,hole,center,sub){
  var tot=parts.reduce(function(s,p){return s+p.v;},0), cx=size/2, cy=size/2, r=size/2-4, a=0, h='';
  if(!tot){ h+='<circle cx="'+cx+'" cy="'+cy+'" r="'+r+'" style="fill:var(--paper-2);stroke:var(--rule)"/>'; }
  parts.forEach(function(p){ if(!p.v) return; var b=a+p.v/tot*Math.PI*2; h+='<path d="'+arcPath(cx,cy,r,a,b)+'" style="fill:'+p.col+';stroke:var(--card);stroke-width:2"><title>'+esc(p.label)+': '+p.v+'</title></path>'; a=b; });
  if(hole) h+='<circle cx="'+cx+'" cy="'+cy+'" r="'+(r*hole)+'" style="fill:var(--card)"/>';
  if(center) h+='<text x="'+cx+'" y="'+(cy-(sub?4:0))+'" text-anchor="middle" dominant-baseline="middle" style="fill:var(--ink);font:700 '+(size>150?22:15)+'px Fraunces,serif">'+center+'</text>';
  if(sub) h+='<text x="'+cx+'" y="'+(cy+16)+'" text-anchor="middle" style="fill:var(--ink-soft);font:600 11px \'Source Sans 3\',sans-serif">'+sub+'</text>';
  return '<svg viewBox="0 0 '+size+' '+size+'" width="'+size+'" height="'+size+'" role="img">'+h+'</svg>';
}
function band(l){ return l===0?'0':(l<=2?'1–2':(l<=4?'3–4':(l<=6?'5–6':'7–8'))); }
function renderReport(){
  var st=critStats(), totC=0, totQ=0;
  st.forEach(function(s){ totC+=s.c; totQ+=s.tot; });
  var h='<div class="theory">';
  h+='<section class="note"><h2>Chapter test report</h2><p class="lt">'+(MODE==='quiz'?'<b>Quiz mode</b> results: answers are marked when you finish each test tab.':'<b>Learning mode</b> results: a question counts as correct only if you got it without the answer being revealed. Switch to Quiz mode for an exam-style score.')+'</p>';
  h+='<div class="rep-top"><div class="rep-pie">'+pieSVG(st.map(function(s){return {v:s.c,col:CRIT_COL[s.letter],label:'Criterion '+s.letter};}),200,0.55,totC+'/'+totQ,'correct')+'</div>';
  h+='<div class="rep-legend"><div class="rep-cap">Marks earned, by criterion</div>'+st.map(function(s){ return '<div class="rep-li"><span class="sw" style="background:'+CRIT_COL[s.letter]+'"></span><b>'+s.letter+'</b>&nbsp;'+esc(s.name)+'<span class="rep-n">'+s.c+' / '+s.tot+'</span></div>'; }).join('')+'</div></div></section>';
  h+='<div class="rep-grid">'+st.map(function(s){
    return '<div class="rep-card"><div class="rep-h"><span class="crit" style="background:'+CRIT_COL[s.letter]+'">'+s.letter+'</span>'+esc(s.name)+'</div>'+
      '<div class="rep-row">'+pieSVG([{v:s.c,col:'var(--success)',label:'Correct'},{v:s.w,col:'var(--danger)',label:'Incorrect'},{v:s.n,col:'var(--locked)',label:'Not attempted'}],120,0.5,s.pct+'%')+
      '<div class="rep-stats"><div><span class="sw" style="background:var(--success)"></span>Correct <b>'+s.c+'</b></div><div><span class="sw" style="background:var(--danger)"></span>Incorrect <b>'+s.w+'</b></div><div><span class="sw" style="background:var(--locked)"></span>Not attempted <b>'+s.n+'</b></div>'+
      '<div class="rep-lvl">Indicative level <b>'+s.lvl+'</b> / 8 <span>(band '+band(s.lvl)+')</span></div></div></div>'+
      '<button class="hub-btn" data-go="'+s.tab+'">Open Test '+s.letter+' →</button></div>';
  }).join('')+'</div>';
  h+='<p class="rep-note">Indicative level = fraction of questions correct × 8, rounded. It is a practice guide only; your teacher awards the real MYP criterion levels.</p></div>';
  var wrap=document.getElementById('wrap'); wrap.innerHTML=h;
  document.querySelector('.slide-progress').style.display='none';
  document.querySelector('.navbar').style.display='none';
  document.getElementById('paletteFab').style.display='none';
  document.getElementById('secSub').textContent='Report — chapter test results by MYP criterion';
  wrap.querySelectorAll('[data-go]').forEach(function(b){ b.addEventListener('click',function(){ activeTab=b.dataset.go; buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); }); });
}
var _rsReport = renderSlide;
renderSlide = function(){
  if(activeTab==='report'){ renderReport(); buildTabbar(); updateChapterProgress(); updateModePill(); saveState(); return; }
  _rsReport();
};


/* ================= skipped-question return ================= */
function firstSkippedIdx(ts){
  for(var i=0;i<ts.items.length;i++){
    var it=ts.items[i];
    if(MODE==='quiz'){ if(!isSlideAttempted(activeTab,i)) return i; }
    else if(it.status==='skipped'||it.status==='unanswered') return i;
  }
  return -1;
}
function skippedList(ts){
  var out=[]; for(var i=0;i<ts.items.length;i++){ var it=ts.items[i];
    if(MODE==='quiz' ? !isSlideAttempted(activeTab,i) : (it.status==='skipped'||it.status==='unanswered')) out.push(i); }
  return out;
}
function renderSkipGate(ts){
  var list=skippedList(ts);
  var h='<div class="qcard done-card"><div class="big">⏭️</div><h2>You skipped '+list.length+' question'+(list.length>1?'s':'')+'</h2>'+
    '<p style="color:var(--ink-soft);font-size:14px;">Answer them now, or submit the tab as it is. Answers are marked only when you submit.</p>'+
    '<div class="skip-chips">'+list.map(function(i){ return '<button class="chip skip-go" data-i="'+i+'">Q'+(i+1)+'</button>'; }).join('')+'</div>'+
    '<div class="skip-btns"><button class="btn btn-primary" id="btnGoSkipped">Answer skipped questions</button><button class="btn" id="btnSubmitAnyway">Submit anyway</button></div></div>';
  document.getElementById('wrap').innerHTML=h;
  document.getElementById('navbarInner').innerHTML='';
  var go=function(i){ ts.idx=i; ts.maxReached=Math.max(ts.maxReached,i); renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); };
  document.getElementById('btnGoSkipped').addEventListener('click',function(){ go(list[0]); });
  document.querySelectorAll('.skip-go').forEach(function(b){ b.addEventListener('click',function(){ go(parseInt(b.dataset.i,10)); }); });
  document.getElementById('btnSubmitAnyway').addEventListener('click',function(){ ts.submitOK=true; renderFinished(); ts.submitOK=false; });
  saveState();
}
var _rfSkip = renderFinished;
renderFinished = function(){
  var ts=state.tabs[activeTab];
  if(MODE==='quiz' && !ts.submitOK && skippedList(ts).length){ renderSkipGate(ts); return; }
  _rfSkip();
  if(MODE==='learning'){
    var list=skippedList(ts);
    if(list.length){
      var card=document.querySelector('.done-card');
      if(card){ var d=document.createElement('div'); d.className='skip-btns';
        d.innerHTML='<button class="btn btn-primary" id="btnGoSkipped">Attempt skipped questions ('+list.length+')</button>';
        card.appendChild(d);
        document.getElementById('btnGoSkipped').addEventListener('click',function(){ ts.idx=list[0]; ts.recorded=false; renderSlide(); window.scrollTo({top:0,behavior:'smooth'}); }); }
    }
  }
};


/* ================= approx answers (physics: within 1%) ================= */
var _amTools = answerMatches;
answerMatches = function(input,answer,accept,expr){
  if(expr==='approx'){
    var n1=parseNum(input); if(n1===null) return false;
    return [answer].concat(accept||[]).some(function(a){ var n2=parseNum(a); if(n2===null) return false; return Math.abs(n1-n2) <= Math.max(0.011*Math.abs(n2), 1e-9); });
  }
  return _amTools(input,answer,accept,expr);
};

/* ================= palette: every question already reached can be opened ================= */
statusOf = function(tabId,i){
  var ts=state.tabs[tabId];
  if(i>ts.maxReached) return 'locked';
  if(i===ts.idx) return 'current';
  var s=ts.items[i].status;
  if(s==='unanswered') return 'skipped';
  return s;
};

/* ================= on-screen keyboard ================= */
var KB={on:true, page:'num', target:null};
try{ var kbp=localStorage.getItem('sopaan-kb-pref'); if(kbp==='off') KB.on=false; }catch(e){}
function kbSave(){ try{ localStorage.setItem('sopaan-kb-pref', KB.on?'on':'off'); }catch(e){} }
var KEYS_NUM=[['7','8','9','(',')'],['4','5','6','−','/'],['1','2','3','.',','],['0',':','%','°','⌫'],['abc','←','→','Clear','Done']];
var KEYS_FRAC=[['7','8','9','/'],['4','5','6','−'],['1','2','3','space'],['0',',','⌫','Clear'],['abc','←','→','Done']];
var KEYS_ALG=[['7','8','9','x','y','n'],['4','5','6','+','−','a'],['1','2','3','×','÷','b'],['0','.','(',')','^','²'],['/','π','|','t','⌫','Clear'],['abc','←','→','Done']];
function kbLayoutFor(inp){ try{ var ts=state.tabs[activeTab], sl=SLIDES[activeTab][ts.idx], stp=sl.flat[parseInt(inp.dataset.step,10)]; var m=stp.expr, key=String(stp.a[inp.dataset.bkey]||'');
  if(m==='words'||m==='glist'||m==='angle') return 'abc'; if(m===true||m==='pow'||m==='primes') return 'alg';
  if(['fv','fe','fl','fm','fi','flist'].indexOf(m)>=0) return 'frac'; if(m==='dec'||m==='approx'||m==='dlist'||m==='set'||m==='coord'||m==='list'||m==='time') return 'num';
  if(/^[\-−]?\d+ \d+\/\d+$/.test(key)||/^[\-−]?\d+\/\d+$/.test(key)) return 'frac'; if(/[a-df-z]/i.test(key.replace(/pi/gi,''))) return /\d/.test(key)?'alg':'abc'; return 'num'; }catch(e){ return 'num'; } }
var KEYS_ABC=[['q','w','e','r','t','y','u','i','o','p'],['a','s','d','f','g','h','j','k','l','-'],['z','x','c','v','b','n','m',',',':','/'],['123','space','←','→','⌫','Done']];
function kbBuild(){
  var el=document.getElementById('vkb'); if(!el){ el=document.createElement('div'); el.id='vkb'; el.className='vkb'; document.body.appendChild(el);
    el.addEventListener('pointerdown',function(e){ var b=e.target.closest('button[data-k]'); e.preventDefault(); if(b) kbPress(b.dataset.k); });
    el.addEventListener('mousedown',function(e){ e.preventDefault(); }); }
  var rows={num:KEYS_NUM,frac:KEYS_FRAC,alg:KEYS_ALG,abc:KEYS_ABC}[KB.page]||KEYS_NUM;
  el.innerHTML='<div class="vkb-top"><span>⌨️ Keyboard</span><button data-k="device" class="vkb-link">Use device keyboard</button></div>'+
    rows.map(function(r){ return '<div class="vkb-row">'+r.map(function(k){
      var cls='vkb-k'+(/^(abc|123|Done|Clear|⌫|←|→|space)$/.test(k)?' fn':'')+(k==='Done'?' done':'');
      return '<button type="button" class="'+cls+'" data-k="'+k+'">'+(k==='space'?'space':k)+'</button>'; }).join('')+'</div>'; }).join('');
  return el;
}
function kbShow(inp){ KB.target=inp; KB.home=kbLayoutFor(inp); KB.page=KB.home; if(!KB.on) return; var cp=document.getElementById('calcPanel'); if(cp&&cp.classList.contains('show')) return; var el=kbBuild(); var nb=document.querySelector('.navbar'); el.style.bottom=(nb?nb.getBoundingClientRect().height:0)+'px'; el.classList.add('show'); document.body.classList.add('kb-open'); setTimeout(function(){ var r=inp.getBoundingClientRect(), top=el.getBoundingClientRect().top; if(r.bottom>top-12) window.scrollBy({top:r.bottom-top+60,behavior:'smooth'}); else if(r.top<60) window.scrollBy({top:r.top-80,behavior:'smooth'}); },30); }
function kbHide(){ var el=document.getElementById('vkb'); if(el) el.classList.remove('show'); document.body.classList.remove('kb-open'); }
function kbInsert(s){
  var t=KB.target; if(!t||t.disabled) return;
  var a=t.selectionStart==null?t.value.length:t.selectionStart, b=t.selectionEnd==null?a:t.selectionEnd;
  t.value=t.value.slice(0,a)+s+t.value.slice(b); var p=a+s.length; try{ t.setSelectionRange(p,p); }catch(e){}
  t.dispatchEvent(new Event('input',{bubbles:true}));
}
function kbPress(k){
  var t=KB.target; if(k==='device'){ KB.on=false; kbSave(); kbHide(); document.querySelectorAll('.blank-input').forEach(function(i){ i.removeAttribute('inputmode'); }); if(t){ t.blur(); setTimeout(function(){ t.focus(); },50); } showToast('Device keyboard on. Tap ⌨️ on any blank to bring the on-screen keyboard back.'); return; }
  if(k==='Done'){ kbHide(); if(t) t.blur(); return; }
  if(k==='abc'||k==='123'){ KB.page=(k==='abc')?'abc':(KB.home&&KB.home!=='abc'?KB.home:'num'); kbBuild(); return; }
  if(!t) return;
  if(k==='⌫'){ var a=t.selectionStart, b=t.selectionEnd; if(a===b&&a>0){ t.value=t.value.slice(0,a-1)+t.value.slice(b); try{t.setSelectionRange(a-1,a-1);}catch(e){} } else { t.value=t.value.slice(0,a)+t.value.slice(b); try{t.setSelectionRange(a,a);}catch(e){} } t.dispatchEvent(new Event('input',{bubbles:true})); return; }
  if(k==='Clear'){ t.value=''; t.dispatchEvent(new Event('input',{bubbles:true})); return; }
  if(k==='←'||k==='→'){ var p=(t.selectionStart||0)+(k==='←'?-1:1); p=Math.max(0,Math.min(t.value.length,p)); try{t.setSelectionRange(p,p);}catch(e){} return; }
  var map={'−':'-','space':' '}; kbInsert(map[k]!==undefined?map[k]:k);
}
function kbWire(){
  document.querySelectorAll('.blank-input').forEach(function(inp){
    if(inp.dataset.kb) return; inp.dataset.kb='1';
    if(KB.on) inp.setAttribute('inputmode','none');
    inp.addEventListener('focus',function(){ kbShow(inp); });
    var btn=document.createElement('button'); btn.type='button'; btn.className='kb-toggle'; btn.title='On-screen keyboard'; btn.textContent='⌨️'; btn.tabIndex=-1;
    btn.addEventListener('mousedown',function(e){ e.preventDefault(); });
    btn.addEventListener('click',function(){ KB.on=true; kbSave(); document.querySelectorAll('.blank-input').forEach(function(i){ i.setAttribute('inputmode','none'); }); inp.focus(); kbShow(inp); });
    if(!inp.disabled) inp.insertAdjacentElement('afterend',btn);
  });
}
document.addEventListener('focusout',function(e){ setTimeout(function(){ var a=document.activeElement; if(!a||!a.classList||!a.classList.contains('blank-input')){ if(!document.querySelector('#vkb:hover')) kbHide(); } },120); });

/* ================= calculator ================= */
var CALC={expr:'', ans:0};
function calcEval(src){
  var s=String(src).replace(/×/g,'*').replace(/÷/g,'/').replace(/−/g,'-').replace(/π/g,'(PI)').replace(/Ans/g,'('+CALC.ans+')').replace(/\^/g,'**').replace(/√\(/g,'sqrt(').replace(/²/g,'**2').replace(/E/g,'*10**');
  s=s.replace(/(^|[(*\/+\-,])-/g,'$1(-1)*');
  if(/[^0-9+\-*/().,a-z A-Z]/.test(s)) throw 0;
  var ok=s.replace(/sin|cos|tan|asin|acos|atan|sqrt|log|ln|PI|abs/g,''); if(/[a-zA-Z]/.test(ok)) throw 0;
  var f=new Function('sin','cos','tan','asin','acos','atan','sqrt','log','ln','PI','abs','return ('+s+');');
  var d=Math.PI/180;
  var v=f(function(x){return Math.sin(x*d);},function(x){return Math.cos(x*d);},function(x){return Math.tan(x*d);},
    function(x){return Math.asin(x)/d;},function(x){return Math.acos(x)/d;},function(x){return Math.atan(x)/d;},Math.sqrt,Math.log10,Math.log,Math.PI,Math.abs);
  if(typeof v!=='number'||!isFinite(v)) throw 0; return v;
}
function fmtNum(v){ if(Math.abs(v)>=1e10||(Math.abs(v)<1e-6&&v!==0)) return v.toExponential(6).replace(/\.?0+e/,'e'); return String(parseFloat(v.toPrecision(10))); }
var CKEYS=[['sin(','cos(','tan(','√(','^','²'],['asin(','acos(','atan(','(',')','π'],['7','8','9','÷','⌫','AC'],['4','5','6','×','E','Ans'],['1','2','3','−','(−)','Insert'],['0','.','=','+']];
function openCalc(){
  var p=document.getElementById('calcPanel');
  if(!p){ p=document.createElement('div'); p.id='calcPanel'; p.className='tool-panel calc';
    p.innerHTML='<div class="tp-head"><b>🧮 Calculator</b><span class="tp-note">degrees · tap a blank, then Insert</span><button class="tp-x" data-c="close">✕</button></div>'+
      '<div class="calc-disp"><div class="calc-expr" id="calcExpr"></div><div class="calc-res" id="calcRes">0</div></div>'+
      '<div class="calc-keys">'+CKEYS.map(function(r){ return r.map(function(k){ return '<button type="button" data-c="'+k+'" class="'+(k==='='?'eq':(/^(AC|⌫|Insert)$/.test(k)?'fn':''))+'">'+({'sin(':'sin','cos(':'cos','tan(':'tan','asin(':'sin⁻¹','acos(':'cos⁻¹','atan(':'tan⁻¹','√(':'√'}[k]||k)+'</button>'; }).join(''); }).join('')+'</div>';
    document.body.appendChild(p);
    p.addEventListener('mousedown',function(e){ if(e.target.closest('button')) e.preventDefault(); });
    p.addEventListener('click',function(e){ var b=e.target.closest('button[data-c]'); if(!b) return; calcKey(b.dataset.c); });
  }
  kbHide(); var nb=document.querySelector('.navbar'); p.style.bottom=((nb?nb.getBoundingClientRect().height:0)+8)+'px'; p.classList.add('show'); document.body.classList.add('calc-open'); calcShow();
}
function calcShow(res){ document.getElementById('calcExpr').textContent=CALC.expr||' '; if(res!==undefined) document.getElementById('calcRes').textContent=res; }
function calcKey(k){
  if(k==='close'){ document.getElementById('calcPanel').classList.remove('show'); document.body.classList.remove('calc-open'); return; }
  if(k==='AC'){ CALC.expr=''; calcShow('0'); return; }
  if(k==='⌫'){ CALC.expr=CALC.expr.replace(/(asin\(|acos\(|atan\(|sin\(|cos\(|tan\(|√\(|Ans|.)$/,''); calcShow(); return; }
  if(k==='='){ try{ var v=calcEval(CALC.expr); CALC.ans=v; calcShow(fmtNum(v)); }catch(e){ calcShow('Error'); } return; }
  if(k==='(−)'){ CALC.expr+='−'; calcShow(); return; }
  if(k==='Insert'){ var r=document.getElementById('calcRes').textContent; if(KB.target && !KB.target.disabled && r!=='Error'){ KB.target.value=r; KB.target.dispatchEvent(new Event('input',{bubbles:true})); showToast('Inserted '+r+' into the blank.'); } else showToast('Tap a blank first, then press Insert.'); return; }
  CALC.expr+=k; calcShow();
}

/* ================= Desmos ================= */
var DESMOS_KEY='dcb31709b452b1cf9dc26972add0fda6', desmosCalc=null, desmosLoading=false;
function openDesmos(exprs){
  var p=document.getElementById('desmosPanel');
  if(!p){ p=document.createElement('div'); p.id='desmosPanel'; p.className='tool-panel desmos';
    p.innerHTML='<div class="tp-head"><b>📈 Desmos graphing calculator</b><button class="tp-x" id="desmosReset">Reset graph</button><button class="tp-x" id="desmosClose">✕</button></div><div id="desmosBox"><div class="desmos-msg">Loading Desmos…</div></div>';
    document.body.appendChild(p);
    document.getElementById('desmosClose').addEventListener('click',function(){ p.classList.remove('show'); });
    document.getElementById('desmosReset').addEventListener('click',function(){ if(desmosCalc) desmosSet(p._exprs||[]); });
  }
  p._exprs=exprs||[]; p.classList.add('show');
  if(window.Desmos){ desmosInit(); desmosSet(p._exprs); return; }
  if(desmosLoading) return; desmosLoading=true;
  var sc=document.createElement('script'); sc.src='https://www.desmos.com/api/v1.9/calculator.js?apiKey='+DESMOS_KEY;
  sc.onload=function(){ desmosInit(); desmosSet(p._exprs); };
  sc.onerror=function(){ desmosLoading=false; document.getElementById('desmosBox').innerHTML='<div class="desmos-msg">Desmos needs an internet connection. <a href="https://www.desmos.com/calculator" target="_blank" rel="noopener">Open Desmos in a new tab</a>.</div>'; };
  document.head.appendChild(sc);
}
function desmosInit(){ if(desmosCalc) return; var box=document.getElementById('desmosBox'); box.innerHTML=''; desmosCalc=Desmos.GraphingCalculator(box,{expressionsCollapsed:false,settingsMenu:false,border:false,degreeMode:true}); }
function desmosSet(exprs){ if(!desmosCalc) return; desmosCalc.setBlank(); (exprs||[]).forEach(function(e,i){ if(typeof e==='string') desmosCalc.setExpression({id:'e'+i,latex:e}); else desmosCalc.setExpression(Object.assign({id:'e'+i},e)); }); }

/* ================= toolbar on each question ================= */
var _rsTools = renderSlide;
renderSlide = function(){
  _rsTools();
  kbHide();
  if(activeTab==='theory'||activeTab==='report') return;
  var ts=state.tabs[activeTab]; if(!ts) return;
  var slide=SLIDES[activeTab][ts.idx], card=document.querySelector('#wrap .qcard');
  if(card && slide && slide.tools && slide.tools.length && !card.querySelector('.tool-bar')){
    var bar=document.createElement('div'); bar.className='tool-bar';
    bar.innerHTML=(slide.tools.indexOf('calc')>=0?'<button type="button" class="tool-btn" data-t="calc">🧮 Calculator</button>':'')+
                  (slide.tools.indexOf('desmos')>=0?'<button type="button" class="tool-btn" data-t="desmos">📈 Desmos graph</button>':'');
    card.insertBefore(bar, card.firstChild);
    bar.addEventListener('click',function(e){ var b=e.target.closest('[data-t]'); if(!b) return; if(b.dataset.t==='calc') openCalc(); else openDesmos(slide.desmos||[]); });
  }
  kbWire();
};



/* ================= signed fractions in text: {−3/4}, {3/−4}, {−2 1/3}, {p/q} ================= */
fr = function(s){ return String(s).replace(/&lt;(\/?)(b|sup|sub|i)>/g,'<$1$2>')
  .replace(/\{([−-]?\d+) (\d+)\/(\d+)\}/g,'<span class="mx">$1<span class="fq"><span>$2</span><span>$3</span></span></span>')
  .replace(/\{([−-]?[A-Za-z0-9]+)\/([−-]?[A-Za-z0-9]+)\}/g,'<span class="fq"><span>$1</span><span>$2</span></span>'); };
/* ================= BMA badge → main page of this sheet (theory hub) ================= */
(function(){
  var c=document.querySelector('.brand .crest'); if(!c) return;
  var a=document.createElement('button'); a.type='button'; a.className='crest crest-home'; a.title='Back to the main page (theory notes)'; a.setAttribute('aria-label','Main page');
  a.innerHTML='BM<span class="crest-h">⌂</span>'; c.parentNode.replaceChild(a,c);
  a.addEventListener('click',function(){
    if(!student||!MODE){ window.scrollTo({top:0,behavior:'smooth'}); return; }
    if(typeof kbHide==='function') kbHide(); if(typeof SP!=='undefined'&&SP.on) spToggle(false);
    var cp=document.getElementById('calcPanel'); if(cp) cp.classList.remove('show'); document.body.classList.remove('calc-open');
    activeTab = (typeof THEORY!=='undefined') ? 'theory' : TAB_DEFS[0].id;
    buildTabbar(); renderSlide(); window.scrollTo({top:0,behavior:'smooth'});
  });
})();

/* ================= session clock: starts at sign-in and keeps running ================= */
var CLK={t0:null};
(function(){
  var row=document.querySelector('.brand .brand-row'); if(!row) return;
  var d=document.createElement('div'); d.className='bmasw off'; d.id='swBox'; d.title='Time since you signed in';
  d.innerHTML='<span class="bmasw-ic">⏱</span><span class="bmasw-t" id="swT">00:00</span>';
  row.appendChild(d); setInterval(swTick,1000);
})();
function swFmt(s){ s=Math.floor(s||0); var h=Math.floor(s/3600), m=Math.floor(s%3600/60), x=s%60; return (h?h+':'+String(m).padStart(2,'0'):String(m).padStart(2,'0'))+':'+String(x).padStart(2,'0'); }
function swTick(){
  var box=document.getElementById('swBox'); if(!box) return;
  if(student && !CLK.t0) CLK.t0=Date.now();
  if(!student) CLK.t0=null;
  box.classList.toggle('off',!CLK.t0);
  document.getElementById('swT').textContent = CLK.t0 ? swFmt((Date.now()-CLK.t0)/1000) : '00:00';
}
function swTab(){ try{ if(!state||!state.tabs||activeTab==='theory'||activeTab==='report') return null; return state.tabs[activeTab]||null; }catch(e){ return null; } }
var _rsSW = renderSlide;
renderSlide = function(){ _rsSW(); swTick(); };
var _rfSW = renderFinished;
renderFinished = function(){ _rfSW(); var done=document.querySelector('#wrap .done-card'); if(CLK.t0 && done && !done.querySelector('.bmasw-done')){ var p=document.createElement('p'); p.className='bmasw-done'; p.textContent='⏱ Time since sign-in: '+swFmt((Date.now()-CLK.t0)/1000); var h2=done.querySelector('h2'); if(h2) h2.insertAdjacentElement('afterend',p); else done.appendChild(p); } };

/* ================= scratchpad (Khan-style, 4 pens) ================= */
var SP={on:false, draw:true, color:'ink', size:3, erase:false, store:{}, key:null, cv:null, ctx:null, down:false, last:null};
var SP_COL={ink:null, blue:'#2563EB', red:'#DC2626', green:'#16A34A'};
function spInk(){ return getComputedStyle(document.documentElement).getPropertyValue('--ink').trim()||'#211E1A'; }
function spKey(){ var ts=swTab(); return ts ? (MODE+'|'+activeTab+'|'+ts.idx) : null; }
function spBuild(){
  if(SP.cv) return;
  var cv=document.createElement('canvas'); cv.id='spCanvas'; cv.className='sp-canvas'; document.body.appendChild(cv);
  var bar=document.createElement('div'); bar.id='spBar'; bar.className='sp-bar';
  bar.innerHTML='<span class="sp-lbl">✏️ Scratchpad</span>'+
    ['ink','blue','red','green'].map(function(c){ return '<button type="button" class="sp-pen" data-c="'+c+'" title="'+(c==='ink'?'black':c)+' pen"><i style="background:'+(c==='ink'?'var(--ink)':SP_COL[c])+'"></i></button>'; }).join('')+
    '<button type="button" class="sp-tool" data-t="erase" title="Eraser">🧽</button><button type="button" class="sp-tool" data-t="size" title="Pen size">●</button><button type="button" class="sp-tool" data-t="scroll" title="Pause drawing to scroll or answer">✋</button>'+
    '<button type="button" class="sp-tool" data-t="clear" title="Clear page">🗑</button><button type="button" class="sp-tool" data-t="close" title="Close scratchpad">✕</button>';
  document.body.appendChild(bar);
  bar.addEventListener('click',function(e){ var b=e.target.closest('button'); if(!b) return;
    if(b.dataset.c){ SP.color=b.dataset.c; SP.erase=false; SP.draw=true; }
    else if(b.dataset.t==='erase'){ SP.erase=true; SP.draw=true; }
    else if(b.dataset.t==='size'){ SP.size = SP.size===3?6:(SP.size===6?1.5:3); b.textContent = SP.size===6?'⬤':(SP.size===1.5?'·':'●'); }
    else if(b.dataset.t==='scroll'){ SP.draw=!SP.draw; }
    else if(b.dataset.t==='clear'){ spClear(); }
    else if(b.dataset.t==='close'){ spToggle(false); return; }
    spUI(); });
  SP.cv=cv; SP.ctx=cv.getContext('2d');
  cv.addEventListener('pointerdown',function(e){ if(!SP.draw) return; e.preventDefault(); cv.setPointerCapture(e.pointerId); SP.down=true; SP.last=spPt(e); spDot(SP.last); });
  cv.addEventListener('pointermove',function(e){ if(!SP.down) return; e.preventDefault(); var p=spPt(e); spLine(SP.last,p); SP.last=p; });
  var up=function(){ if(SP.down){ SP.down=false; spSave(); } };
  cv.addEventListener('pointerup',up); cv.addEventListener('pointercancel',up); cv.addEventListener('pointerleave',up);
  window.addEventListener('resize',function(){ if(SP.on) spFit(true); });
}
function spPt(e){ var r=SP.cv.getBoundingClientRect(); return {x:e.clientX-r.left, y:e.clientY-r.top}; }
function spStyle(){ var c=SP.ctx; c.lineCap='round'; c.lineJoin='round'; c.globalCompositeOperation=SP.erase?'destination-out':'source-over'; c.strokeStyle=c.fillStyle=(SP.color==='ink'?spInk():SP_COL[SP.color]); c.lineWidth=SP.erase?22:SP.size; }
function spDot(p){ spStyle(); var c=SP.ctx; c.beginPath(); c.arc(p.x,p.y,(SP.erase?11:SP.size/2),0,Math.PI*2); c.fill(); }
function spLine(a,b){ spStyle(); var c=SP.ctx; c.beginPath(); c.moveTo(a.x,a.y); c.lineTo(b.x,b.y); c.stroke(); }
function spSave(){ if(SP.key&&SP.cv){ try{ SP.store[SP.key]=SP.cv.toDataURL(); }catch(e){} } }
function spClear(){ if(!SP.ctx) return; SP.ctx.save(); SP.ctx.setTransform(1,0,0,1,0,0); SP.ctx.clearRect(0,0,SP.cv.width,SP.cv.height); SP.ctx.restore(); if(SP.key) delete SP.store[SP.key]; }
function spFit(keep){
  var wrap=document.getElementById('wrap'); if(!wrap||!SP.cv) return;
  var r=wrap.getBoundingClientRect(), top=r.top+window.scrollY, h=Math.max(wrap.scrollHeight, window.innerHeight-r.top)+40, w=document.documentElement.clientWidth;
  var old=keep&&SP.key?SP.store[SP.key]:null, dpr=window.devicePixelRatio||1;
  SP.cv.style.top=top+'px'; SP.cv.style.left='0px'; SP.cv.style.width=w+'px'; SP.cv.style.height=h+'px';
  SP.cv.width=Math.round(w*dpr); SP.cv.height=Math.round(h*dpr); SP.ctx.setTransform(dpr,0,0,dpr,0,0);
  if(old){ var im=new Image(); im.onload=function(){ SP.ctx.save(); SP.ctx.setTransform(1,0,0,1,0,0); SP.ctx.drawImage(im,0,0); SP.ctx.restore(); }; im.src=old; }
}
function spLoad(){ spSave(); SP.key=spKey(); spFit(false); var d=SP.key&&SP.store[SP.key]; if(d){ var im=new Image(); im.onload=function(){ SP.ctx.save(); SP.ctx.setTransform(1,0,0,1,0,0); SP.ctx.drawImage(im,0,0); SP.ctx.restore(); }; im.src=d; } }
function spUI(){
  var bar=document.getElementById('spBar'); if(!bar) return;
  bar.querySelectorAll('.sp-pen').forEach(function(b){ b.classList.toggle('on', !SP.erase && SP.draw && b.dataset.c===SP.color); });
  bar.querySelector('[data-t=erase]').classList.toggle('on', SP.erase && SP.draw);
  bar.querySelector('[data-t=scroll]').classList.toggle('on', !SP.draw);
  SP.cv.classList.toggle('passive', !SP.draw);
  var fb=document.getElementById('spFab'); if(fb) fb.classList.toggle('on',SP.on);
}
function spToggle(on){
  spBuild(); SP.on=(on===undefined)?!SP.on:on;
  document.body.classList.toggle('sp-open',SP.on); SP.cv.style.display=SP.on?'block':'none'; document.getElementById('spBar').style.display=SP.on?'flex':'none';
  if(SP.on){ SP.draw=true; SP.erase=false; spLoad(); if(typeof kbHide==='function') kbHide(); } else { spSave(); }
  spUI();
}
(function(){ var f=document.createElement('button'); f.type='button'; f.id='spFab'; f.className='sp-fab'; f.innerHTML='✏️ <span>Scratchpad</span>'; f.title='Open a scratchpad to write your working'; f.addEventListener('click',function(){ spToggle(); }); document.body.appendChild(f); })();
var _rsSP = renderSlide;
renderSlide = function(){ _rsSP(); var fab=document.getElementById('spFab'); var q=!!swTab(); if(fab) fab.style.display=q?'':'none'; if(!q && SP.on) spToggle(false); if(SP.on) setTimeout(spLoad,30); };

renderLogin();
})();
</script>
</body>
</html>
