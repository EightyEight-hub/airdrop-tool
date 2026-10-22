<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover" />
<title>Airdrop Studio</title>
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
  textarea{min-height:100px;font-family:ui-monospace,Menlo,monospace;font-size:13px}
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
       color:var(--dim);max-height:240px;overflow:auto}
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
<h1>Airdrop Studio</h1>

<div class="card" id="resumecard" style="display:none">
  <h2>Unfinished job</h2>
  <div id="resumeinfo"></div>
  <div class="row">
    <button id="resume">Resume</button>
    <button id="discard" class="sec">Discard</button>
  </div>
</div>

<div class="card">
  <h2>1 · Wallet</h2>
  <div class="kv"><span>Status</span><span id="wstat">not connected</span></div>
  <div class="kv"><span>Account</span><span id="wacct" class="mono">—</span></div>
  <div class="kv"><span>Chain</span><span id="wchain">—</span></div>
  <div class="row">
    <button id="connect">Connect</button>
    <button id="switch" class="sec">Switch to Base</button>
  </div>
</div>

<div class="card">
  <h2>2 · Harvest addresses <span class="pill">optional</span></h2>
  <label>Any busy token address to pull active wallets from</label>
  <input id="h_token" placeholder="0x… source token" autocapitalize="off" autocorrect="off" spellcheck="false" />
  <button id="harvest" class="sec">Fetch addresses</button>
  <div class="kv"><span>Fetched</span><span id="hcount">0</span></div>
</div>

<div class="card">
  <h2>3 · Deploy your token</h2>
  <div class="kv"><span>Compiler</span><span id="cstat">idle</span></div>
  <label>Name</label><input id="d_name" placeholder="Aurora Cat" />
  <label>Symbol</label><input id="d_symbol" placeholder="AURA" autocapitalize="characters" />
  <div class="row">
    <div><label>Supply (whole)</label><input id="d_supply" value="21000000" inputmode="numeric" /></div>
    <div><label>Tax (bps)</label><input id="d_tax" value="0" inputmode="numeric" /></div>
  </div>
  <label>Treasury (blank = your address)</label>
  <input id="d_treasury" placeholder="0x…" autocapitalize="off" autocorrect="off" spellcheck="false" />
  <button id="deploy">Compile &amp; deploy</button>
  <div class="row">
    <button id="rebuild" class="sec">Rebuild</button>
    <button id="copysrc" class="sec">Copy source</button>
  </div>
</div>

<div class="card">
  <h2>4 · Token</h2>
  <label>Contract address (auto-filled after deploy)</label>
  <input id="token" placeholder="0x…" autocapitalize="off" autocorrect="off" spellcheck="false" />
  <button id="load" class="sec">Load token</button>
  <div id="tinfo"></div>
  <button id="pool" class="sec">Copy address &amp; open Aerodrome</button>
</div>

<div class="card">
  <h2>5 · Recipients</h2>
  <label>Addresses — plain list or raw JSON, both work</label>
  <textarea id="addrs" placeholder="0xabc…&#10;0xdef…"></textarea>
  <div class="row">
    <button id="paste" class="sec">Paste</button>
    <button id="clear" class="sec">Clear</button>
  </div>
  <div class="kv"><span>Valid unique</span><span id="naddr">0</span></div>
</div>

<div class="card">
  <h2>6 · Options</h2>
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
/* ================= embedded contract ================= */
const SOURCE = `
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract AirdropToken {
    string public name;
    string public symbol;
    uint8  public constant decimals = 18;
    uint256 public totalSupply;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    uint256 public constant MAX_TAX_BPS = 1000;
    uint256 public taxBps;
    address public treasury;
    mapping(address => bool) public taxExempt;
    address public owner;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Airdropped(uint256 recipients, uint256 amountEach);

    modifier onlyOwner() { require(msg.sender == owner, "not owner"); _; }

    constructor(string memory n, string memory s, uint256 supply, uint256 tax, address tr) {
        require(tax <= MAX_TAX_BPS, "tax too high");
        name = n; symbol = s; taxBps = tax; owner = msg.sender;
        treasury = tr == address(0) ? msg.sender : tr;
        uint256 t = supply * 10 ** decimals;
        totalSupply = t; balanceOf[msg.sender] = t;
        emit Transfer(address(0), msg.sender, t);
        taxExempt[msg.sender] = true;
        taxExempt[treasury] = true;
    }

    function approve(address sp, uint256 v) external returns (bool) {
        allowance[msg.sender][sp] = v; emit Approval(msg.sender, sp, v); return true;
    }
    function transfer(address to, uint256 v) external returns (bool) {
        _transfer(msg.sender, to, v); return true;
    }
    function transferFrom(address f, address to, uint256 v) external returns (bool) {
        uint256 a = allowance[f][msg.sender];
        if (a != type(uint256).max) { require(a >= v, "allowance"); allowance[f][msg.sender] = a - v; }
        _transfer(f, to, v); return true;
    }

    function airdrop(address[] calldata recipients, uint256 each) external onlyOwner {
        uint256 n = recipients.length; uint256 total = each * n;
        uint256 bal = balanceOf[msg.sender];
        require(bal >= total, "insufficient balance");
        balanceOf[msg.sender] = bal - total;
        for (uint256 i; i < n; ++i) {
            balanceOf[recipients[i]] += each;
            emit Transfer(msg.sender, recipients[i], each);
        }
        emit Airdropped(n, each);
    }

    function setTax(uint256 t) external onlyOwner { require(t <= MAX_TAX_BPS, "too high"); taxBps = t; }
    function setTreasury(address t) external onlyOwner { treasury = t; }
    function setTaxExempt(address a, bool e) external onlyOwner { taxExempt[a] = e; }
    function transferOwnership(address n) external onlyOwner { owner = n; taxExempt[n] = true; }

    function _transfer(address f, address to, uint256 v) internal {
        require(to != address(0), "to = zero");
        uint256 bal = balanceOf[f];
        require(bal >= v, "balance too low");
        uint256 fee;
        if (taxBps > 0 && !taxExempt[f] && !taxExempt[to]) fee = (v * taxBps) / 10000;
        if (fee > 0) {
            balanceOf[f] = bal - v; balanceOf[treasury] += fee; balanceOf[to] += v - fee;
            emit Transfer(f, treasury, fee); emit Transfer(f, to, v - fee);
        } else {
            balanceOf[f] = bal - v; balanceOf[to] += v; emit Transfer(f, to, v);
        }
    }
}
`;

/* ================= helpers ================= */
const $ = id => document.getElementById(id);
const log = (m, c='') => { const el=$('log'); el.innerHTML += `\n<span class="${c}">${m}</span>`; el.scrollTop = el.scrollHeight; };
const short = a => a ? a.slice(0,6)+'…'+a.slice(-4) : '—';
const JOBKEY = 'airdrop_job_v3';
const BUILDKEY = 'solc_build_v1';

const CHAINS = { 1:'Ethereum', 8453:'Base', 137:'Polygon' };
const SCAN = { 8453:'https://base.blockscout.com', 1:'https://eth.blockscout.com', 137:'https://polygon.blockscout.com' };

const ABI = [
  'function name() view returns (string)',
  'function symbol() view returns (string)',
  'function decimals() view returns (uint8)',
  'function totalSupply() view returns (uint256)',
  'function balanceOf(address) view returns (uint256)',
  'function owner() view returns (address)',
  'function airdrop(address[] recipients, uint256 amountEach)',
];

let provider, signer, account, chainId, token, decimals = 18, symbol = '', plan = null, solcReady = false;
const tokenAddr = () => $('token').value.trim();

/* ================= compiler ================= */
function setCstat(t, c=''){ $('cstat').textContent = t; $('cstat').className = c; }

function loadCompiler(){
  if (solcReady) return Promise.resolve();
  setCstat('downloading…', 'warn');
  return new Promise((resolve, reject) => {
    window.Module = window.Module || {};
    window.Module.onRuntimeInitialized = () => { solcReady = true; setCstat('ready','ok'); resolve(); };
    const s = document.createElement('script');
    s.src = 'https://binaries.soliditylang.org/bin/soljson-v0.8.24+commit.e11b9ed9.js';
    s.onerror = () => { setCstat('failed','bad'); reject(new Error('compiler download failed')); };
    document.head.appendChild(s);
    const poll = setInterval(() => {
      if (window.Module && window.Module.calledRun && !solcReady){
        clearInterval(poll); solcReady = true; setCstat('ready','ok'); resolve();
      }
    }, 300);
  });
}

function compileContract(){
  const compile = window.Module.cwrap('solidity_compile', 'string', ['string','number']);
  const input = {
    language:'Solidity',
    sources:{ 'AirdropToken.sol':{ content: SOURCE } },
    settings:{ optimizer:{enabled:false,runs:200},
               outputSelection:{ '*':{ '*':['abi','evm.bytecode.object'] } } }
  };
  const out = JSON.parse(compile(JSON.stringify(input), 0));
  const errs = (out.errors || []).filter(e => e.severity === 'error');
  if (errs.length) throw new Error(errs.map(e => e.formattedMessage).join('\n'));
  const c = out.contracts['AirdropToken.sol']['AirdropToken'];
  return { abi: c.abi, bytecode: '0x' + c.evm.bytecode.object };
}

async function getArtifact(){
  try{
    const c = JSON.parse(localStorage.getItem(BUILDKEY));
    if (c && c.src === SOURCE){ setCstat('cached','ok'); log('using cached build ✓','ok'); return c; }
  }catch{}
  if (!solcReady) await loadCompiler();
  log('compiling…');
  const art = compileContract();
  localStorage.setItem(BUILDKEY, JSON.stringify({ src: SOURCE, ...art }));
  setCstat('cached','ok');
  log('compiled & cached ✓','ok');
  return art;
}

async function deployToken(){
  if (!signer){ log('connect wallet first','bad'); return; }
  try{
    const { abi, bytecode } = await getArtifact();
    const name = $('d_name').value.trim() || 'Airdrop Token';
    const sym  = $('d_symbol').value.trim() || 'AIR';
    const sup  = $('d_supply').value.trim() || '21000000';
    const tax  = Number($('d_tax').value || 0);
    const tr   = $('d_treasury').value.trim() || account;
    log('deploying — confirm in MetaMask…');
    const f = new ethers.ContractFactory(abi, bytecode, signer);
    const c = await f.deploy(name, sym, BigInt(sup), tax, tr);
    log('tx → ' + c.deploymentTransaction().hash);
    await c.waitForDeployment();
    const addr = await c.getAddress();
    log('deployed → ' + addr, 'ok');
    $('token').value = addr;
    await loadToken();
  }catch(e){ log('deploy failed: ' + (e.shortMessage || e.message), 'bad'); }
}

/* ================= harvest (keyless, Blockscout) ================= */
async function harvest(){
  const src = $('h_token').value.trim();
  if (!ethers.isAddress(src)){ log('invalid source token','bad'); return; }
  const base = SCAN[chainId];
  if (!base){ log('harvest works on Base / Ethereum / Polygon only','warn'); return; }
  try{
    let url = `${base}/api/v2/tokens/${src}/transfers`;
    const found = new Set();
    for (let p = 0; p < 4; p++){
      log(`fetching page ${p+1}…`);
      const j = await (await fetch(url)).json();
      const items = j.items || [];
      for (const it of items){
        if (it.from?.hash) found.add(it.from.hash);
        if (it.to?.hash)   found.add(it.to.hash);
      }
      $('hcount').textContent = found.size;
      if (!j.next_page_params) break;
      url = `${base}/api/v2/tokens/${src}/transfers?` + new URLSearchParams(j.next_page_params);
    }
    found.delete('0x0000000000000000000000000000000000000000');
    found.delete(src.toLowerCase());
    const list = [...found].map(a => ethers.getAddress(a.toLowerCase()));
    $('addrs').value = list.join('\n');
    parseAddresses();
    log(`harvested ${list.length} addresses ✓`, 'ok');
  }catch(e){ log('harvest failed: ' + (e.message||e), 'bad'); }
}

/* ================= resume marker ================= */
const getJob   = () => { try { return JSON.parse(localStorage.getItem(JOBKEY)); } catch { return null; } };
const saveJob  = j  => localStorage.setItem(JOBKEY, JSON.stringify(j));
const clearJob = () => localStorage.removeItem(JOBKEY);
function refreshBanner(){
  const j = getJob();
  if (!j){ $('resumecard').style.display = 'none'; return; }
  $('resumecard').style.display = '';
  $('resumeinfo').innerHTML = `
    <div class="kv"><span>Token</span><span class="mono">${short(j.token)}</span></div>
    <div class="kv"><span>Progress</span><span>${j.done} / ${j.list.length} sent</span></div>`;
}

/* ================= wallet ================= */
async function connect(){
  if (!window.ethereum){ log('No wallet. Open inside MetaMask\'s browser.','bad'); return; }
  try{
    provider = new ethers.BrowserProvider(window.ethereum);
    account  = (await provider.send('eth_requestAccounts', []))[0];
    signer   = await provider.getSigner();
    chainId  = Number((await provider.getNetwork()).chainId);
    $('wacct').textContent  = short(account);
    $('wstat').textContent  = 'connected'; $('wstat').className = 'ok';
    $('wchain').textContent = CHAINS[chainId] || ('chain ' + chainId);
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

/* ================= token ================= */
async function loadToken(){
  const a = tokenAddr();
  if (!ethers.isAddress(a)){ log('invalid token address','bad'); return; }
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
      <div class="kv"><span>Total supply</span><span>${ethers.formatUnits(ts, decimals)}</span></div>
      <div class="kv"><span>Your balance</span><span>${ethers.formatUnits(bal, decimals)}</span></div>
      <div class="kv"><span>You are owner</span><span class="${isOwn?'ok':'bad'}">${isOwn?'yes':'no'}</span></div>`;
    log('loaded ' + sy, 'ok');
  }catch(e){ log('load failed: ' + (e.shortMessage||e.message), 'bad'); }
}

/* ================= addresses ================= */
function parseAddresses(){
  const found = $('addrs').value.match(/0x[a-fA-F0-9]{40}/g) || [];
  const seen = new Set(), out = [];
  for (const a of found){
    const n = ethers.getAddress(a.toLowerCase());
    if (!seen.has(n)){ seen.add(n); out.push(n); }
  }
  $('naddr').textContent = out.length;
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

/* ================= preview ================= */
async function preview(){
  if (!signer){ log('connect wallet first','bad'); return; }
  if (!ethers.isAddress(tokenAddr())){ log('load a token first','bad'); return; }
  token = new ethers.Contract(tokenAddr(), ABI, signer);
  let list = parseAddresses();
  if (!list.length){ log('no valid addresses','bad'); return; }
  log(`parsed ${list.length} unique addresses`);
  if ($('skip').checked){ log('checking holders…'); list = await filterHolders(list); log(`kept ${list.length}`); }
  if (!list.length){ log('nothing to send','warn'); return; }

  let amountEach;
  try{ amountEach = ethers.parseUnits($('amount').value.trim(), decimals); }
  catch{ log('bad amount','bad'); return; }

  const total = amountEach * BigInt(list.length);
  const bal   = await token.balanceOf(account);
  const bs    = Math.max(1, parseInt($('batch').value) || 150);
  const okBal = bal >= total;

  $('summary').innerHTML = `
    <div class="kv"><span>Recipients</span><span>${list.length}</span></div>
    <div class="kv"><span>Total needed</span><span>${ethers.formatUnits(total, decimals)} ${symbol}</span></div>
    <div class="kv"><span>Your balance</span><span class="${okBal?'ok':'bad'}">${ethers.formatUnits(bal, decimals)} ${symbol}</span></div>
    <div class="kv"><span>Batches</span><span>${Math.ceil(list.length/bs)} × ≤${bs}</span></div>`;

  plan = { list, amountEach, bs, startAt: 0 };
  $('send').disabled = !okBal;
  log(okBal ? 'ready' : 'insufficient balance', okBal ? 'ok' : 'bad');
}

/* ================= send ================= */
async function send(){
  if (!plan) return;
  $('send').disabled = true;
  const { list, amountEach, bs } = plan;
  const gas = BigInt($('gas').value || 6000000);
  const dry = $('dry').checked;
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
      if (!dry) break;
    }
  }
  if (dry) log('simulation complete — no gas spent','warn');
  else if ((getJob()?.done || 0) >= list.length){ log('all done.','ok'); clearJob(); refreshBanner(); }
  $('send').disabled = false;
}

async function resumeJob(){
  const j = getJob();
  if (!j) return;
  if (!signer) await connect();
  if (Number(j.chainId) !== chainId){ log('wrong chain','bad'); return; }
  if (j.account.toLowerCase() !== account.toLowerCase()){ log('wrong account','bad'); return; }
  token    = new ethers.Contract(j.token, ABI, signer);
  decimals = Number(await token.decimals());
  symbol   = await token.symbol();
  $('token').value = j.token; $('amount').value = j.amount; $('batch').value = j.bs; $('gas').value = j.gas;
  plan = { list: j.list, amountEach: ethers.parseUnits(j.amount, decimals), bs: j.bs, startAt: j.done };
  log(`resuming at batch ${Math.floor(j.done / j.bs) + 1}`, 'warn');
  await send();
}

/* ================= wiring ================= */
$('connect').onclick = connect;
$('switch').onclick  = switchBase;
$('harvest').onclick = harvest;
$('deploy').onclick  = deployToken;
$('load').onclick    = loadToken;
$('preview').onclick = preview;
$('send').onclick    = send;
$('resume').onclick  = resumeJob;
$('discard').onclick = () => { clearJob(); refreshBanner(); };
$('clear').onclick   = () => { $('addrs').value = ''; parseAddresses(); };
$('paste').onclick   = async () => {
  try{ $('addrs').value += await navigator.clipboard.readText(); parseAddresses(); }
  catch{ log('clipboard blocked — paste manually','warn'); }
};
$('rebuild').onclick = () => { localStorage.removeItem(BUILDKEY); setCstat('idle'); log('cache cleared — will recompile next deploy'); };
$('copysrc').onclick = async () => {
  try{ await navigator.clipboard.writeText(SOURCE); log('contract source copied — paste it on Basescan to verify','ok'); }
  catch{ log('copy failed','warn'); }
};
$('pool').onclick = async () => {
  if (!ethers.isAddress(tokenAddr())){ log('no token address yet','warn'); return; }
  try{ await navigator.clipboard.writeText(tokenAddr()); log('token address copied — paste it in Aerodrome','ok'); }catch{}
  window.open('https://aerodrome.finance/liquidity', '_blank');
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
