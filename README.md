# Index.html
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXORA — личный помощник в поиске коммерческого помещения</title>
<style>
  :root{
    --bg:#ffffff;
    --fg:#0a0a0a;
    --muted:#8a8a8a;
    --line:#e6e6e6;
    --soft:#f5f5f5;
  }
  *{margin:0;padding:0;box-sizing:border-box}
  html{scroll-behavior:smooth}
  body{
    font-family:-apple-system,BlinkMacSystemFont,"Helvetica Neue","Segoe UI",Arial,sans-serif;
    background:var(--bg);color:var(--fg);
    -webkit-font-smoothing:antialiased;
    overflow-x:hidden;
  }
  button{font-family:inherit;cursor:pointer}
  input,select{font-family:inherit}

  /* ============ HEADER ============ */
  header{
    position:fixed;top:0;left:0;right:0;height:64px;z-index:50;
    display:grid;grid-template-columns:1fr auto 1fr;align-items:center;
    padding:0 14px;
    background:rgba(255,255,255,.88);
    backdrop-filter:blur(12px);-webkit-backdrop-filter:blur(12px);
    border-bottom:1px solid var(--line);
  }
  .header-left{display:flex;align-items:center;gap:2px;justify-self:start}
  .icon-btn{
    width:42px;height:42px;display:grid;place-items:center;
    background:none;border:none;border-radius:12px;color:var(--fg);
    transition:background .2s, transform .15s;
  }
  .icon-btn:hover{background:var(--soft)}
  .icon-btn:active{transform:scale(.94)}
  .brand{
    font-size:14px;font-weight:700;letter-spacing:.42em;text-indent:.42em;
    cursor:pointer;user-select:none;white-space:nowrap;
  }

  /* ============ DRAWER ============ */
  #overlay{
    position:fixed;inset:0;background:rgba(0,0,0,.35);
    opacity:0;pointer-events:none;transition:opacity .35s;z-index:70;
  }
  #overlay.open{opacity:1;pointer-events:auto}

  #drawer{
    position:fixed;top:0;left:0;bottom:0;width:330px;max-width:86vw;z-index:80;
    background:var(--bg);border-right:1px solid var(--line);
    transform:translateX(-100%);
    transition:transform .38s cubic-bezier(.4,0,.2,1);
    display:flex;flex-direction:column;padding:20px 16px;
  }
  #drawer.open{transform:translateX(0)}
  .drawer-head{
    display:flex;align-items:center;justify-content:space-between;
    padding:4px 6px 20px;border-bottom:1px solid var(--line);margin-bottom:16px;
  }
  .drawer-brand{font-size:12px;font-weight:700;letter-spacing:.4em;text-indent:.4em}
  .nav{display:flex;flex-direction:column;gap:2px}
  .nav-item{
    display:flex;align-items:center;justify-content:space-between;gap:12px;
    width:100%;padding:15px 14px;border:none;border-radius:12px;
    background:none;color:var(--fg);font-size:15.5px;text-align:left;
    transition:background .18s,color .18s;
  }
  .nav-item span.arrow{opacity:0;transition:opacity .18s;font-size:13px}
  .nav-item:hover,.nav-item.active{background:var(--fg);color:#fff}
  .nav-item:hover span.arrow,.nav-item.active span.arrow{opacity:.6}
  .drawer-foot{
    margin-top:auto;padding:16px 8px 4px;border-top:1px solid var(--line);
    font-size:11px;letter-spacing:.18em;color:var(--muted);
  }

  /* ============ LAYOUT ============ */
  main{padding-top:64px}
  .page{display:none}
  .page.active{display:block;animation:fade .35s ease}
  @keyframes fade{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:none}}

  .wrap{max-width:1000px;margin:0 auto;padding:0 24px}
  .page-head{padding:64px 0 40px;border-bottom:1px solid var(--line);margin-bottom:44px}
  .label{
    font-size:11px;letter-spacing:.28em;color:var(--muted);
    text-transform:uppercase;margin-bottom:14px;
  }
  h2.title{font-size:clamp(26px,4.4vw,42px);font-weight:600;letter-spacing:-.02em;line-height:1.1}
  .lead{color:var(--muted);font-size:15.5px;line-height:1.6;margin-top:14px;max-width:560px}

  /* ============ HERO ============ */
  .hero{
    position:relative;min-height:calc(100vh - 64px);
    display:flex;flex-direction:column;align-items:center;justify-content:center;
    text-align:center;padding:80px 24px 120px;overflow:hidden;
  }
  .hero-logo{
    position:absolute;inset:0;display:grid;place-items:center;
    pointer-events:none;z-index:0;
  }
  .hero-logo svg{
    width:min(78vmin,720px);height:auto;
    stroke:#000;stroke-width:9;fill:none;opacity:.055;
  }
  .hero-inner{position:relative;z-index:1;max-width:860px}
  .hero-brand{
    display:block;font-size:clamp(38px,8vw,78px);font-weight:700;
    letter-spacing:.34em;text-indent:.34em;line-height:1;margin-bottom:26px;
  }
  .hero h1{
    font-size:clamp(19px,2.8vw,30px);font-weight:400;line-height:1.35;
    letter-spacing:-.01em;color:var(--fg);
  }
  .hero-sub{
    margin-top:26px;font-size:14px;letter-spacing:.22em;
    color:var(--muted);text-transform:uppercase;
  }
  .scroll-hint{
    position:absolute;bottom:34px;left:50%;transform:translateX(-50%);
    font-size:20px;color:var(--muted);animation:bob 2s infinite;
  }
  @keyframes bob{0%,100%{transform:translate(-50%,0)}50%{transform:translate(-50%,7px)}}

  /* ============ ABOUT ============ */
  .about{padding:96px 0 120px;border-top:1px solid var(--line)}
  .about h3{font-size:clamp(22px,3.2vw,32px);font-weight:600;letter-spacing:-.02em;max-width:640px;line-height:1.25}
  .about p.intro{color:var(--muted);font-size:16px;line-height:1.65;max-width:620px;margin-top:20px}
  .features{display:grid;grid-template-columns:repeat(3,1fr);gap:1px;background:var(--line);margin-top:56px;border:1px solid var(--line)}
  .feature{background:var(--bg);padding:32px 26px;transition:background .25s,color .25s}
  .feature:hover{background:var(--fg);color:#fff}
  .feature:hover .f-num{color:rgba(255,255,255,.5)}
  .feature:hover .f-text{color:rgba(255,255,255,.72)}
  .f-num{font-size:11px;letter-spacing:.24em;color:var(--muted);margin-bottom:26px}
  .f-title{font-size:17px;font-weight:600;margin-bottom:10px}
  .f-text{font-size:14px;line-height:1.6;color:var(--muted)}

  /* ============ CARDS / LISTS ============ */
  .cat-grid{
    display:grid;grid-template-columns:repeat(auto-fill,minmax(180px,1fr));
    gap:1px;background:var(--line);border:1px solid var(--line);margin-bottom:56px;
  }
  .cat{
    background:var(--bg);padding:28px 22px;border:none;text-align:left;
    display:flex;flex-direction:column;gap:20px;
    transition:background .22s,color .22s;
  }
  .cat:hover,.cat.active{background:var(--fg);color:#fff}
  .cat svg{stroke:currentColor;fill:none;stroke-width:1.4;stroke-linecap:round;stroke-linejoin:round}
  .cat-name{font-size:15px;font-weight:500;letter-spacing:-.01em}
  .cat-hint{font-size:11px;letter-spacing:.16em;opacity:.55;text-transform:uppercase}

  /* form */
  .form-grid{display:grid;grid-template-columns:1fr 1fr;gap:20px 24px;max-width:720px}
  .field{display:flex;flex-direction:column;gap:9px}
  .field.full{grid-column:1/-1}
  .field label{font-size:11px;letter-spacing:.2em;color:var(--muted);text-transform:uppercase}
  .field input,.field select{
    width:100%;padding:14px 16px;font-size:15px;color:var(--fg);
    background:var(--bg);border:1px solid var(--line);border-radius:12px;
    outline:none;transition:border-color .2s;
    appearance:none;-webkit-appearance:none;
  }
  .field select{background-image:url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 24 24' fill='none' stroke='%230a0a0a' stroke-width='2' stroke-linecap='round'><path d='M6 9l6 6 6-6'/></svg>");background-repeat:no-repeat;background-position:right 16px center;padding-right:42px;cursor:pointer}
  .field input:focus,.field select:focus{border-color:var(--fg)}

  .btn{
    display:inline-flex;align-items:center;justify-content:center;gap:10px;
    padding:15px 30px;font-size:14px;font-weight:600;letter-spacing:.06em;
    background:var(--fg);color:#fff;border:1px solid var(--fg);border-radius:12px;
    transition:background .2s,color .2s,transform .12s;
  }
  .btn:hover{background:#fff;color:var(--fg)}
  .btn:active{transform:scale(.98)}
  .btn.ghost{background:none;color:var(--fg)}
  .btn.ghost:hover{background:var(--fg);color:#fff}
  .btn-row{margin-top:32px}

  .result{
    margin-top:28px;padding:18px 20px;border:1px solid var(--line);
    border-radius:12px;font-size:14.5px;display:none;
  }
  .result.show{display:block;animation:fade .3s}
  .result b{font-weight:600}

  /* list cards */
  .list{display:flex;flex-direction:column;gap:1
