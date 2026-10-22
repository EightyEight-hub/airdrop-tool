<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover" />
<title>Airdrop Tool</title>
<style>
  :root{--bg:#0d0f12;--card:#161a20;--line:#242a33;--txt:#e7ecf3;--dim:#8b97a8;
        --accent:#3b82f6;--ok:#22c55e;--warn:#f59e0b;--bad:#ef4444}
  *{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
  body{margin:0;background:var(--bg);color:var(--txt);
       font:16px/1.5 -apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,sans-serif;
       padding:16px 14px calc(92px + env(safe-area-inset-bottom))}
  h1{font-size:20px;margin:4px 0 16px}
  h2{font-size:13px;text-transform:uppercase;letter-spacing:.06em;color:var(--dim);
     margin:0 0 10px;font-weight:600}
  .card{background:var(--card);border:1px solid var(--line);border-radius:14px;
        padding:14px;margin-bottom:14px}
  label{display:block;font-size:13px;color:var(--dim);margin:10px 0 4px}
  input,textarea{width:100%;background:#0b0e12;color:var(--txt);border:1px solid var(--line);
    border-radius:10px;padding:12px;font-size:16px;font-family:inherit}
  textarea{min-height:104px;font-family:ui-monospace,Menlo,monospace;font-size:13px}
  button{background:var(--accent);color:#fff;border:0;border-radius:10px;padding:13px 16px;
    font-size:16px;font-weight:600;width:100%;margin-top:10px}
  button.sec{background:#222933;color:var(--txt)}
  button:disabled{opacity:.45}
  .row{display:flex;gap:10px}.row>*{flex:1}
  .kv{display:flex;justify-content:space-between;gap:12px;padding:6px 0;
      border-bottom:1px solid var(--line);font-size:14px}
  .kv:last-child{border-bottom:0}
  .kv span:first-child{color:var(--dim)}
  .mono{font-family:ui-monospace,Menlo,monospace;font-size:12px;word-break:break-all}
  #log{white-space:pre-wrap;font-family:ui-monospace,Menlo,monospace;font-size:12px;
       color:var(--dim);max-height:260px;overflow:auto}
  .ok{color:var(--ok)}.bad{color:var(--bad)}.warn{color:var(--warn)}
  .bar{position:fixed;left:0;right:0;bottom:0;background:rgba(13,15,18,.94);
       border-top:1px solid var(--line);padding:12px 14px calc(12px + env(safe-area-inset-bottom));
       backdrop-filter:blur(8px)}
  .bar button{margin:0}
  .pill{display:inline-block;font-size:12px;padding:3px 9px;border-radius:99px;
        background:#222933;color:var(--dim)}
  .check{display:flex;align-items:center;gap:10px;margin-top:14px}
  .check input{width:auto}
  .check span{color:var(--txt);font-size:14px}
</style>
</head>
<body>
<h1>Airdrop Tool</h1>

<div class="card" id="resumecard" style="display:none">
  <h2>Unfinished job</h2>
  <div id="resumeinfo"></div>
  <div class="row">
    <button id="resume">Resume</button>
    <button id="discard" class="sec">Discard</button>
  </div>
</div>

<div class="card">
  <h2>Wallet</h2>
  <div class="kv"><span>Status</span><span id="wstat">not connected</span></div>
  <div class="kv"><span>Account</span><span id="wacct" class="mono">—</span></div>
  <div class="kv"><span>Chain</span><span id="wchain">—</span></div>
  <div class="row">
    <button id="connect">Connect</button>
    <button id="switch" class="sec">Switch to Base</button>
  </div>
</div>

<div class="card">
  <h2>Token</h2>
  <label>Token contract address</label>
  <input id="token" placeholder="0x…" autocapitalize="off" autocorrect="off" spellcheck="false" />
  <button id="load" class="sec">Load token</button>
  <div id="tinfo"></div>
</div>

<div class="card">
  <h2>Recipients</h2>
  <label>Addresses — one per line, or comma/space separated</label>
  <textarea id="addrs" placeholder="0xabc…&#10;0xdef…"></textarea>
  <div class="row">
    <button id="paste" class="sec">Paste</button>
    <button id="clear" class="sec">Clear</button>
  </div>
  <div class="kv"><span>Valid unique</span><span id="naddr">0</span></div>
</div>

<div class="card">
  <h2>Options</h2>
  <div class="row">
    <div><label>Amount each</label><input id="amount" value="100" inputmode="decimal" /></div>
    <div><label>Batch size</label><input id="batch" value="150" inputmode="numeric" /></div>
  </div>
  <label>Gas limit per batch</label>
  <input id="gas" value="6000000" inputmode="numeric" />
  <label class="check"><input type="checkbox" id="skip" checked /><span>Skip wallets that already hold this token</span></label>
  <label class="check"><input type="checkbox" id="dry" /><span>Dry run — simulate, spend no gas</span></label>
</div>

<div class="card">
  <h2>Summary</h2>
  <div id="summary"><span class="pill">nothing prepared</span></div>
  <button id="preview" class="sec">Preview</button>
</div>

<div class="card">
  <h2>Log</h2>
  <div id="log">ready.</div>
</div>

<div class="bar"><button id="send" disabled>Send airdrop</button></div>

<script src="https://cdn.jsdelivr.net/npm/ethers@6.13.4/dist/ethers.umd.min.js"></script>
<script>
const $ = id => document.getElementById(id);
const log = (m, c='') => { const el=$('log'); el.innerHTML += `\n<span class="${c}">${m}</span>`; el.scrollTop = el.scrollHeight; };
const short = a => a ? a.slice(0,6)+'…'+a.slice(-4) : '—';
const JOBKEY = 'airdrop_job_v1';

const CHAINS = {
  1:    {name:'Ethereum', hex:'0x1',    cur:{name:'Ether',symbol:'ETH',decimals:18}},
  8453: {name:'Base',     hex:'0x2105', cur:{name:'Ether',symbol:'ETH',decimals:18}},
  137:  {name:'Polygon',  hex:'0x89',   cur:{name:'POL',symbol:'POL',decimals:18}},
};

const ABI = [
  'function name() view returns (string)',
  'function symbol() view returns (string)',
  'function decimals() view returns (uint8)',
  'function totalSupply() view returns (uint256)',
  'function balanceOf(address) view returns (uint256)',
  'function owner() view returns (address)',
  'function airdrop(address[] recipients, uint256 amountEach)',
];

let provider, signer, account, chainId, token, decimals = 18, symbol = '', plan = null;
const tokenAddr = () => $('token').value.trim();

/* ---------- resume marker ---------- */
const getJob   = () => { try { return JSON.parse(localStorage.getItem(JOBKEY)); } catch { return null; } };
const saveJob  = j  => localStorage.setItem(JOBKEY, JSON.stringify(j));
const clearJob = () => localStorage.removeItem(JOBKEY);

function refreshBanner(){
  const j = getJob();
  if (!j){ $('resumecard').style.display = 'none'; return; }
  $('resumecard').style.display = '';
  $('resumeinfo').innerHTML = `
    <div class="kv"><span>Token</span><span class="mono">${short(j.token)}</span></div>
    <div class="kv"><span>Progress</span><span>${j.done} / ${j.list.length} sent</span></div>
    <div class="kv"><span>Account</span><span class="mono">${short(j.account)}</span></div>`;
}

/* ---------- wallet ---------- */
async function connect(){
  if (!window.ethereum){ log('No wallet. Open this page inside MetaMask\'s browser.', 'bad'); return; }
  try{
    provider = new ethers.BrowserProvider(window.ethereum);
    account  = (await provider.send('eth_requestAccounts', []))[0];
    signer   = await provider.getSigner();
    chainId  = Number((await provider.getNetwork()).chainId);
    $('wacct').textContent  = short(account);
    $('wstat').textContent  = 'connected'; $('wstat').className = 'ok';
    $('wchain').textContent = CHAINS[chainId]?.name || ('chain ' + chainId);
    log('connected ' + short(account), 'ok');
    if (tokenAddr()) await loadToken();
  }catch(e){ log('connect failed: ' + (e.shortMessage||e.message), 'bad'); }
}

async function switchBase(){
  try{
    await window.ethereum.request({ method:'wallet_switchEthereumChain', params:[{chainId:'0x2105'}] });
  }catch(e){
    if (e.code === 4902){
      await window.ethereum.request({ method:'wallet_addEthereumChain', params:[{
        chainId:'0x2105', chainName:'Base',
        nativeCurrency:{name:'Ether',symbol:'ETH',decimals:18},
        rpcUrls:['https://mainnet.base.org'], blockExplorerUrls:['https://basescan.org']
      }]});
    } else { log('switch failed: ' + (e.shortMessage||e.message), 'bad'); return; }
  }
  await connect();
}

/* ---------- token ---------- */
async function loadToken(){
  const a = tokenAddr();
  if (!ethers.isAddress(a)){ log('invalid token address', 'bad'); return; }
  try{
    const c = new ethers.Contract(a, ABI, signer || provider);
    const [nm, sy, dc, ts] = await Promise.all([
      c.name().catch(()=>'?'), c.symbol().catch(()=>'?'),
      c.decimals().catch(()=>18), c.totalSupply().catch(()=>0n)
    ]);
    decimals = Number(dc); symbol = sy; token = c;
    const bal   = account ? await c.balanceOf(account) : 0n;
    const owner = await c.owner().catch(()=>null);
    const isOwn = owner && account && owner.toLowerCase() === account.toLowerCase();
    $('tinfo').innerHTML = `
      <div class="kv"><span>Name</span><span>${nm}</span></div>
      <div class="kv"><span>Symbol</span><span>${sy}</span></div>
      <div class="kv"><span>Decimals</span><span>${decimals}</span></div>
      <div class="kv"><span>Total supply</span><span>${ethers.formatUnits(ts, decimals)}</span></div>
      <div class="kv"><span>Your balance</span><span>${ethers.formatUnits(bal, decimals)}</span></div>
      <div class="kv"><span>Owner</span><span class="mono">${owner?short(owner):'—'}</span></div>
      <div class="kv"><span>You are owner</span><span class="${isOwn?'ok':'bad'}">${isOwn?'yes':'no'}</span></div>`;
    log('loaded ' + sy, 'ok');
  }catch(e){ log('load failed: ' + (e.shortMessage||e.message), 'bad'); }
}

/* ---------- addresses ---------- */
function parseAddresses(){
  const raw = $('addrs').value.split(/[\s,;]+/).map(s=>s.trim()).filter(Boolean);
  const seen = new Set(), out = []; let bad = 0;
  for (const a of raw){
    try{ const n = ethers.getAddress(a.toLowerCase()); if(!seen.has(n)){ seen.add(n); out.push(n); } }
    catch{ bad++; }
  }
  $('naddr').textContent = out.length + (bad ? ` (+${bad} invalid)` : '');
  return out;
}

async function filterHolders(list){
  const out = new Array(list.length); let next = 0, done = 0;
  async function worker(){
    for(;;){
      const i = next++;
      if (i >= list.length) return;
      let keep = true;
      try{ keep = (await token.balanceOf(list[i])) === 0n; }catch{}
      out[i] = keep ? list[i] : null;
      if (++done % 100 === 0) log(`  checked ${done}/${list.length}`);
    }
  }
  await Promise.all(Array.from({length:4}, worker));
  return out.filter(Boolean);
}

/* ---------- preview ---------- */
async function preview(){
  if (!signer){ log('connect wallet first', 'bad'); return; }
  if (!ethers.isAddress(tokenAddr())){ log('load a token first', 'bad'); return; }
  token = new ethers.Contract(tokenAddr(), ABI, signer);
  let list = parseAddresses();
  if (!list.length){ log('no valid addresses', 'bad'); return; }
  log(`parsed ${list.length} unique addresses`);

  if ($('skip').checked){
    log('checking holders…');
    list = await filterHolders(list);
    log(`kept ${list.length} non-holders`);
  }
  if (!list.length){ log('nothing to send', 'warn'); return; }

  let amountEach;
  try{ amountEach = ethers.parseUnits($('amount').value.trim(), decimals); }
  catch{ log('bad amount', 'bad'); return; }

  const total = amountEach * BigInt(list.length);
  const bal   = await token.balanceOf(account);
  const bs    = Math.max(1, parseInt($('batch').value) || 150);
  const okBal = bal >= total;

  $('summary').innerHTML = `
    <div class="kv"><span>Recipients</span><span>${list.length}</span></div>
    <div class="kv"><span>Per wallet</span><span>${$('amount').value} ${symbol}</span></div>
    <div class="kv"><span>Total needed</span><span>${ethers.formatUnits(total, decimals)} ${symbol}</span></div>
    <div class="kv"><span>Your balance</span><span class="${okBal?'ok':'bad'}">${ethers.formatUnits(bal, decimals)} ${symbol}</span></div>
    <div class="kv"><span>Batches</span><span>${Math.ceil(list.length/bs)} × ≤${bs}</span></div>`;

  plan = { list, amountEach, bs, startAt: 0 };
  $('send').disabled = !okBal;
  log(okBal ? 'ready' : 'insufficient balance', okBal ? 'ok' : 'bad');
}

/* ---------- send / simulate ---------- */
async function send(){
  if (!plan) return;
  $('send').disabled = true;
  const { list, amountEach, bs } = plan;
  const gas  = BigInt($('gas').value || 6000000);
  const dry  = $('dry').checked;
  const total = Math.ceil(list.length / bs);
  let done = dry ? 0 : (plan.startAt || 0);

  if (!dry){
    saveJob({ token: tokenAddr(), chainId, account, amount: $('amount').value,
              bs, gas: $('gas').value, list, done });
    refreshBanner();
  }

  for (let i = done; i < list.length; i += bs){
    const n = Math.floor(i / bs) + 1;
    const chunk = list.slice(i, i + bs);
    try{
      if (dry){
        await token.airdrop.staticCall(chunk, amountEach, { gasLimit: gas });
        log(`batch ${n}/${total} OK (dry run)`, 'ok');
      } else {
        const tx = await token.airdrop(chunk, amountEach, { gasLimit: gas });
        log(`batch ${n}/${total} sent → ${tx.hash}`);
        const rc = await tx.wait();
        log(`  confirmed block ${rc.blockNumber}`, 'ok');
        const j = getJob(); if (j){ j.done = i + bs; saveJob(j); refreshBanner(); }
      }
    }catch(e){
      log(`batch ${n}/${total} FAILED: ${e.shortMessage || e.message}`, 'bad');
      if (!dry) break;                     // stop so you can resume later
    }
  }

  if (dry){ log('simulation complete — no gas spent', 'warn'); }
  else if (done >= list.length || getJob()?.done >= list.length){
    log('all batches complete.', 'ok'); clearJob(); refreshBanner();
  }
  $('send').disabled = false;
}

/* ---------- resume ---------- */
async function resumeJob(){
  const j = getJob();
  if (!j) return;
  if (!signer) await connect();
  if (Number(j.chainId) !== chainId){ log('wrong chain — switch to the right network first', 'bad'); return; }
  if (j.account.toLowerCase() !== account.toLowerCase()){ log('connect the job\'s account first', 'bad'); return; }
  token    = new ethers.Contract(j.token, ABI, signer);
  decimals = Number(await token.decimals());
  symbol   = await token.symbol();
  $('token').value  = j.token;
  $('amount').value = j.amount;
  $('batch').value  = j.bs;
  $('gas').value    = j.gas;
  plan = { list: j.list, amountEach: ethers.parseUnits(j.amount, decimals), bs: j.bs, startAt: j.done };
  log(`resuming at batch ${Math.floor(j.done / j.bs) + 1}`, 'warn');
  await send();
}

/* ---------- wiring ---------- */
$('connect').onclick = connect;
$('switch').onclick  = switchBase;
$('load').onclick    = loadToken;
$('preview').onclick = preview;
$('send').onclick    = send;
$('resume').onclick  = resumeJob;
$('discard').onclick = () => { clearJob(); refreshBanner(); log('job discarded'); };
$('clear').onclick   = () => { $('addrs').value = ''; parseAddresses(); };
$('paste').onclick   = async () => {
  try{ $('addrs').value += await navigator.clipboard.readText(); parseAddresses(); }
  catch{ log('clipboard blocked — paste manually', 'warn'); }
};
$('addrs').addEventListener('input', parseAddresses);
$('dry').onchange = () => { $('send').textContent = $('dry').checked ? 'Simulate (no gas)' : 'Send airdrop'; };

if (window.ethereum){
  window.ethereum.on?.('accountsChanged', () => connect());
  window.ethereum.on?.('chainChanged',   () => connect());
}
refreshBanner();
</script>
</body>
</html>
