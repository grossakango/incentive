# incentive
訪問看護のインセンティブを計算するソフトです。

<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:var(--font-sans);color:var(--color-text-primary);font-size:14px}
.tabs{display:flex;gap:6px;margin-bottom:16px;padding-bottom:10px;border-bottom:0.5px solid var(--color-border-tertiary)}
.tab{padding:5px 14px;border-radius:var(--border-radius-md);border:0.5px solid var(--color-border-tertiary);background:var(--color-background-secondary);cursor:pointer;font-size:13px;color:var(--color-text-secondary)}
.tab.on{background:var(--color-background-primary);color:var(--color-text-primary);border-color:var(--color-border-primary);font-weight:500}
.slbl{font-size:11px;font-weight:500;color:var(--color-text-secondary);border-bottom:0.5px solid var(--color-border-tertiary);margin:14px 0 8px;padding-bottom:4px;display:flex;justify-content:space-between;align-items:center}
.cgrid{display:grid;grid-template-columns:repeat(auto-fill,minmax(122px,1fr));gap:7px;margin-bottom:2px}
.vc{background:var(--color-background-primary);border:0.5px solid var(--color-border-tertiary);border-radius:var(--border-radius-md);padding:10px 10px 8px;display:flex;flex-direction:column;gap:2px;transition:border-color .12s,background .12s}
.vc.act-n{border-color:#185FA5;background:#e6f1fb18}
.vc.act-iryo{border-color:#B06000;background:#faeeda18}
.vc.act-mp{border-color:#5C50C6;background:#eeedfe18}
.vc.act-mv{border-color:#237B4B;background:#e1f5ee18}
.vc.act-ojt{border-color:var(--color-border-secondary)}
.vn{font-size:13px;font-weight:500;line-height:1.3}
.vs{font-size:11px;color:var(--color-text-secondary);line-height:1.4;margin-top:1px}
.vf{font-size:11px;color:var(--color-text-secondary);margin-top:1px}
.vic{font-size:10px;min-height:14px;margin-top:2px}
.ctr{display:flex;align-items:center;gap:6px;margin-top:6px}
.cb{width:26px;height:26px;border-radius:50%;border:0.5px solid var(--color-border-secondary);background:var(--color-background-secondary);cursor:pointer;font-size:15px;display:flex;align-items:center;justify-content:center;flex-shrink:0;user-select:none}
.cb:hover{background:var(--color-background-tertiary)}
.cv{font-size:16px;font-weight:500;min-width:22px;text-align:center}
.hrow{display:flex;gap:8px;align-items:flex-end;flex-wrap:wrap;margin-bottom:14px}
.fd{display:flex;flex-direction:column;gap:3px}
.fd label{font-size:11px;color:var(--color-text-secondary)}
input,select{padding:6px 10px;border-radius:var(--border-radius-md);border:0.5px solid var(--color-border-secondary);background:var(--color-background-primary);color:var(--color-text-primary);font-size:13px}
.btn{padding:6px 14px;border-radius:var(--border-radius-md);border:0.5px solid var(--color-border-secondary);background:var(--color-background-primary);color:var(--color-text-primary);cursor:pointer;font-size:13px}
.btn:hover{background:var(--color-background-secondary)}
.bp{background:#185FA5;color:white;border-color:#185FA5}.bp:hover{background:#0C447C}
.bd{background:var(--color-background-danger);color:var(--color-text-danger);border-color:var(--color-border-danger)}
.bg{background:#237B4B;color:white;border-color:#237B4B}.bg:hover{background:#1a5c37}
.prev{background:var(--color-background-secondary);border-radius:var(--border-radius-lg);padding:14px 16px;display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin:14px 0}
.rrow{display:flex;align-items:center;gap:8px;padding:10px 14px;background:var(--color-background-primary);border:0.5px solid var(--color-border-tertiary);border-radius:var(--border-radius-md);margin-bottom:6px;flex-wrap:wrap}
.card{background:var(--color-background-primary);border:0.5px solid var(--color-border-tertiary);border-radius:var(--border-radius-lg);padding:16px;margin-bottom:12px}
.mrow{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:14px}
.mt{background:var(--color-background-secondary);border-radius:var(--border-radius-md);padding:10px 14px;flex:1;min-width:100px}
.mtl{font-size:11px;color:var(--color-text-secondary);margin-bottom:3px}
.mtv{font-size:18px;font-weight:500}
table{width:100%;border-collapse:collapse;font-size:12px}
th{background:var(--color-background-secondary);padding:7px 8px;text-align:right;font-weight:500;color:var(--color-text-secondary);white-space:nowrap}
th:first-child{text-align:left}
td{padding:7px 8px;text-align:right;border-bottom:0.5px solid var(--color-border-tertiary)}
td:first-child{text-align:left}
tfoot td{background:var(--color-background-secondary);font-weight:500;border-bottom:none}
.empty{text-align:center;color:var(--color-text-secondary);padding:24px;font-size:13px}
.toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);background:#237B4B;color:white;padding:8px 20px;border-radius:var(--border-radius-md);font-size:13px;font-weight:500;opacity:0;transition:opacity .3s;pointer-events:none;z-index:999;white-space:nowrap}
.toast.show{opacity:1}
</style>

<div id="toast" class="toast">保存しました ✓</div>
<div style="padding:1rem 0">
<div style="font-size:15px;font-weight:500;margin-bottom:14px">インセンティブ自動計算</div>
<div class="tabs">
  <button class="tab on" onclick="sw('input',this)">入力</button>
  <button class="tab" onclick="sw('records',this)">記録一覧</button>
  <button class="tab" onclick="sw('summary',this)">月次集計</button>
  <button class="tab" onclick="sw('settings',this)">設定</button>
</div>
<div id="pane-input"></div>
<div id="pane-records" style="display:none"></div>
<div id="pane-summary" style="display:none"></div>
<div id="pane-settings" style="display:none"></div>
</div>

<script>
let UID=1;
const VTYPES=[
  {k:'KI2', g:'介護保険',   lbl:'Ⅰ-2',         sub:'30分未満 471単位',       def:4710,  mode:'n',  cs:'n'},
  {k:'KI3', g:'介護保険',   lbl:'Ⅰ-3',         sub:'30〜60分 823単位',       def:8230,  mode:'n',  cs:'n'},
  {k:'KI4', g:'介護保険',   lbl:'Ⅰ-4',         sub:'60〜90分 1128単位',      def:11280, mode:'n',  cs:'n'},
  {k:'KI5', g:'介護保険',   lbl:'Ⅰ-5',         sub:'リハビリ 20分 294単位',  def:2940,  mode:'n',  cs:'n'},
  {k:'KI5x2',g:'介護保険',  lbl:'Ⅰ-5×2',       sub:'リハビリ 40分 588単位',  def:5880,  mode:'n',  cs:'n'},
  {k:'IM',  g:'医療保険',   lbl:'医療保険',      sub:'固定 8,550円',           def:8550,  mode:'n',  cs:'iryo'},
  {k:'MP_K30',g:'複数名訪問',lbl:'複数名 介護\n30分未満',sub:'介護基本＋加算 254単位', def:4710, mpA:2540,mode:'mp',cs:'mp'},
  {k:'MP_K60',g:'複数名訪問',lbl:'複数名 介護\n30分以上',sub:'介護基本＋加算 402単位', def:8230, mpA:4020,mode:'mp',cs:'mp'},
  {k:'MP_I', g:'複数名訪問', lbl:'複数名 医療',  sub:'医療基本＋加算 450点',   def:8550,  mpA:4500,mode:'mp',cs:'mp'},
  {k:'MV2', g:'複数回加算',  lbl:'複数回\n1日2回',sub:'加算 450点',            def:4500,  mode:'mv', cs:'mv'},
  {k:'MV3', g:'複数回加算',  lbl:'複数回\n1日3回',sub:'加算 800点',            def:8000,  mode:'mv', cs:'mv'},
  {k:'OJT', g:'その他',      lbl:'OJT・同行',   sub:'除外（時給のみ）',       def:0,     mode:'ojt',cs:'ojt'},
];
const GROUP_NOTES={
  '介護保険':'単価は設定タブで変更可',
  '医療保険':'月初・週何日目問わず固定',
  '複数名訪問':'規程第7条: 自動50%分配 / 同行スタッフ側でも同じボタンを押す',
  '複数回加算':'加算×75%をICに加算 / 通常訪問も上で別途カウント',
  'その他':''
};
const S={
  staff:[],records:[],cfg:{fees:{}},
  f:{eid:null,date:'',sid:'',hrs:'',cnts:{},ocv:[],ocIn:''},
  sy:new Date().getFullYear(),sm:new Date().getMonth()+1,
  ns:'',nr:''
};
function mkCnts(){const o={};VTYPES.forEach(v=>{o[v.k]=0;});return o;}
S.f.cnts=mkCnts();

function getFee(k,sub){
  const v=VTYPES.find(x=>x.k===k);if(!v)return 0;
  if(sub==='mpA')return S.cfg.fees[k+'_mpA']??v.mpA??0;
  return S.cfg.fees[k]??v.def??0;
}
function icPer(k){
  const v=VTYPES.find(x=>x.k===k);if(!v)return 0;
  const f=getFee(k);
  if(v.mode==='n') return Math.floor(f*0.5);
  if(v.mode==='mp'){const mpa=getFee(k,'mpA');return Math.floor((f*0.5+mpa*0.75*0.5)/2);}
  if(v.mode==='mv') return Math.floor(f*0.75);
  return 0;
}
function calcDay(rec){
  const st=S.staff.find(s=>s.id===rec.sid);if(!st||!rec.hrs)return null;
  const hw=Math.floor(rec.hrs*st.rate);
  let ib=0;
  VTYPES.forEach(v=>{const c=rec.cnts[v.k]||0;if(c&&v.mode!=='ojt')ib+=c*icPer(v.k);});
  const ob=Math.floor((rec.ocv||[]).reduce((s,o)=>s+o.a*0.75,0));
  const ia=ib>hw?ib-hw:0,rp=Math.max(hw,ib);
  return{hw,ib,ia,ob,rp,tp:rp+ob};
}
function formCalc(){return calcDay({sid:+S.f.sid,hrs:+S.f.hrs,cnts:S.f.cnts,ocv:S.f.ocv});}

function sw(tab,el){
  ['input','records','summary','settings'].forEach(t=>document.getElementById('pane-'+t).style.display=t===tab?'block':'none');
  document.querySelectorAll('.tab').forEach(b=>b.classList.remove('on'));el.classList.add('on');
  ({input:ri,records:rr,summary:rs,settings:rset})[tab]();
}

function advanceDate(d){
  if(!d)return '';
  const dt=new Date(d);dt.setDate(dt.getDate()+1);
  return dt.toISOString().split('T')[0];
}

const IC_COLOR={n:'#185FA5',iryo:'#B06000',mp:'#5C50C6',mv:'#237B4B',ojt:'var(--color-text-secondary)'};

function ri(){
  const groups={};VTYPES.forEach(v=>{if(!groups[v.g])groups[v.g]=[];groups[v.g].push(v);});
  const sopts=S.staff.map(s=>`<option value="${s.id}"${s.id==S.f.sid?' selected':''}>${s.name}</option>`).join('');
  const c=formCalc();
  let h=`<div class="hrow">
    <div class="fd"><label>日付</label><input type="date" value="${S.f.date}" oninput="S.f.date=this.value;up()"></div>
    <div class="fd"><label>スタッフ</label><select onchange="S.f.sid=this.value;up()"><option value="">選択...</option>${sopts}</select></div>
    <div class="fd"><label>勤務時間合計（h）</label><input type="number" step="0.25" placeholder="例: 6.5" value="${S.f.hrs}" style="width:110px" oninput="S.f.hrs=this.value;up()"></div>
  </div>`;

  Object.entries(groups).forEach(([g,vs])=>{
    h+=`<div class="slbl"><span>${g}</span><span style="font-weight:400;font-size:10px">${GROUP_NOTES[g]||''}</span></div><div class="cgrid">`;
    vs.forEach(v=>{
      const cnt=S.f.cnts[v.k]||0,ic=icPer(v.k),f=getFee(v.k);
      const actCls=cnt>0?'act-'+v.cs:'';
      const col=IC_COLOR[v.cs]||'var(--color-text-secondary)';
      let icTxt=v.mode==='ojt'?'除外（時給のみ）':v.mode==='mp'?`IC/人 ¥${ic.toLocaleString()}`:v.mode==='mv'?`IC ¥${ic.toLocaleString()}`:`IC/件 ¥${ic.toLocaleString()}`;
      let feeDisplay=v.mode==='mp'
        ?`<div class="vf">基本¥${f.toLocaleString()}+加算¥${getFee(v.k,'mpA').toLocaleString()}</div>`
        :f?`<div class="vf">¥${f.toLocaleString()}</div>`:`<div class="vf" style="height:15px"></div>`;
      h+=`<div class="vc${actCls?' '+actCls:''}">
        <div class="vn">${v.lbl.replace('\n','<br>')}</div>
        <div class="vs">${v.sub}</div>
        ${feeDisplay}
        <div class="ctr">
          <button class="cb" onclick="adj('${v.k}',-1)">−</button>
          <span class="cv" style="color:${cnt>0?col:'var(--color-text-primary)'}">${cnt}</span>
          <button class="cb" onclick="adj('${v.k}',1)">＋</button>
        </div>
        <div class="vic" style="color:${col}">${icTxt}</div>
      </div>`;
    });
    h+=`</div>`;
  });

  h+=`<div class="slbl"><span>オンコール緊急</span><span style="font-weight:400;font-size:10px">売上×75% 独立計算（第4条）</span></div>
  <div style="display:flex;gap:8px;align-items:center;margin-bottom:8px;flex-wrap:wrap">
    <input type="number" placeholder="緊急訪問の売上額（円）" style="width:200px" value="${S.f.ocIn}" oninput="S.f.ocIn=this.value">
    <button class="btn" onclick="addOC()">＋ 追加</button>
  </div>
  <div style="margin-bottom:8px">${S.f.ocv.map(o=>`
    <span style="display:inline-flex;align-items:center;gap:4px;padding:3px 8px;border-radius:var(--border-radius-md);font-size:11px;background:var(--color-background-danger);color:var(--color-text-danger);margin:0 4px 4px 0">
      緊急¥${o.a.toLocaleString()} → 報酬¥${Math.floor(o.a*0.75).toLocaleString()}
      <span onclick="rmOC(${o.id})" style="cursor:pointer;font-weight:700;margin-left:2px">×</span>
    </span>`).join('')}</div>`;

  h+=`<div class="prev" id="pv">${pvHTML(c)}</div>`;
  h+=`<div style="display:flex;gap:8px;margin-top:4px;align-items:center;flex-wrap:wrap">
    <button class="btn bp" onclick="saveRec(false)">${S.f.eid?'更新':'保存（日付を維持）'}</button>
    ${!S.f.eid?`<button class="btn bg" onclick="saveRec(true)">保存して翌日へ</button>`:''}
    ${S.f.eid?`<button class="btn" onclick="cancelEd()">キャンセル</button>`:''}
    <button class="btn" onclick="softClr()" style="color:var(--color-text-secondary)">訪問だけリセット</button>
    <button class="btn" onclick="fullClr()" style="color:var(--color-text-secondary);font-size:12px">全リセット</button>
  </div>
  <div style="margin-top:8px;font-size:11px;color:var(--color-text-secondary)">
    💡 保存後もスタッフ・日付・勤務時間は維持されます。「保存して翌日へ」で日付だけ1日進みます。
  </div>`;
  document.getElementById('pane-input').innerHTML=h;
}

function pvHTML(c){
  if(!c)return`<div style="grid-column:1/4;color:var(--color-text-secondary);font-size:13px;text-align:center;padding:4px 0">スタッフと勤務時間を入力すると計算プレビューが表示されます</div>`;
  return`<div class="mt"><div class="mtl">時給合計</div><div class="mtv">¥${c.hw.toLocaleString()}</div></div>
  <div class="mt"><div class="mtl">IC基礎額</div><div class="mtv" style="color:#185FA5">¥${c.ib.toLocaleString()}</div></div>
  <div class="mt"><div class="mtl">支給見込</div><div class="mtv" style="color:#185FA5">¥${c.tp.toLocaleString()}</div>
    ${c.ia>0?`<div style="font-size:10px;color:#185FA5">IC加算 +¥${c.ia.toLocaleString()}</div>`:''}
    ${c.ob>0?`<div style="font-size:10px;color:var(--color-text-danger)">緊急報酬 +¥${c.ob.toLocaleString()}</div>`:''}
  </div>`;
}
function up(){const pv=document.getElementById('pv');if(pv)pv.innerHTML=pvHTML(formCalc());}
function adj(k,d){S.f.cnts[k]=Math.max(0,(S.f.cnts[k]||0)+d);ri();}
function addOC(){const a=+S.f.ocIn;if(!a)return;S.f.ocv.push({id:UID++,a});S.f.ocIn='';ri();}
function rmOC(id){S.f.ocv=S.f.ocv.filter(o=>o.id!==id);ri();}

function showToast(msg){
  const t=document.getElementById('toast');t.textContent=msg||'保存しました ✓';
  t.classList.add('show');setTimeout(()=>t.classList.remove('show'),2000);
}

function saveRec(nextDay){
  if(!S.f.date||!S.f.sid||!S.f.hrs){alert('日付・スタッフ・勤務時間を入力してください');return;}
  const rec={id:S.f.eid||UID++,date:S.f.date,sid:+S.f.sid,hrs:+S.f.hrs,cnts:{...S.f.cnts},ocv:[...S.f.ocv]};
  if(S.f.eid){S.records=S.records.map(r=>r.id===S.f.eid?rec:r);S.f.eid=null;}
  else S.records.push(rec);
  // 保持: sid, hrs はそのまま。cnts と ocv のみリセット。日付は選択による
  const newDate=nextDay?advanceDate(S.f.date):S.f.date;
  S.f.cnts=mkCnts();S.f.ocv=[];S.f.ocIn='';
  S.f.date=newDate;
  showToast(nextDay?`保存 → ${newDate} へ ✓`:'保存しました ✓');
  ri();
}
// 訪問カウントとオンコールのみリセット、スタッフ・日付・勤務時間は維持
function softClr(){S.f.cnts=mkCnts();S.f.ocv=[];S.f.ocIn='';ri();}
// 全リセット
function fullClr(){S.f={eid:null,date:'',sid:'',hrs:'',cnts:mkCnts(),ocv:[],ocIn:''};ri();}
function cancelEd(){S.f.eid=null;softClr();}

function editRec(id){
  const r=S.records.find(x=>x.id===id);if(!r)return;
  const cnts=mkCnts();Object.assign(cnts,r.cnts);
  S.f={eid:r.id,date:r.date,sid:String(r.sid),hrs:String(r.hrs),cnts,ocv:[...r.ocv],ocIn:''};
  sw('input',document.querySelectorAll('.tab')[0]);ri();
}
function delRec(id){S.records=S.records.filter(r=>r.id!==id);rr();}

function rr(){
  const el=document.getElementById('pane-records');
  if(!S.records.length){el.innerHTML='<div class="empty">記録がありません</div>';return;}
  let h='';
  [...S.records].sort((a,b)=>b.date.localeCompare(a.date)).forEach(r=>{
    const st=S.staff.find(s=>s.id===r.sid),c=calcDay(r);
    const vs=VTYPES.filter(v=>(r.cnts[v.k]||0)>0).map(v=>`${v.lbl.replace('\n',' ')}×${r.cnts[v.k]}`).join('・')+(r.ocv?.length?'・緊急×'+r.ocv.length:'');
    h+=`<div class="rrow">
      <span style="font-weight:500;min-width:88px">${r.date}</span>
      <span>${st?.name||'不明'}</span>
      <span style="color:var(--color-text-secondary);font-size:12px">${r.hrs}h</span>
      <span style="color:var(--color-text-secondary);font-size:12px;flex:1">${vs||'訪問なし'}</span>
      ${c?`<span style="font-weight:500;color:#185FA5">¥${c.tp.toLocaleString()}</span>`:''}
      <button class="btn" onclick="editRec(${r.id})" style="font-size:12px;padding:4px 10px">編集</button>
      <button class="btn bd" onclick="delRec(${r.id})" style="font-size:12px;padding:4px 10px">削除</button>
    </div>`;
  });
  el.innerHTML=h;
}

// ===== 月次集計 =====
function rs(){
  const el=document.getElementById('pane-summary');
  const yopts=[2025,2026,2027].map(y=>`<option${y===S.sy?' selected':''} value="${y}">${y}年</option>`).join('');
  const mopts=Array.from({length:12},(_,i)=>i+1).map(m=>`<option${m===S.sm?' selected':''} value="${m}">${m}月</option>`).join('');
  const recs=S.records.filter(r=>{if(!r.date)return false;const d=new Date(r.date);return d.getFullYear()===S.sy&&d.getMonth()+1===S.sm;});
  const byStaff={};
  recs.forEach(r=>{
    const st=S.staff.find(s=>s.id===r.sid);if(!st)return;
    if(!byStaff[r.sid])byStaff[r.sid]={id:r.sid,name:st.name,days:[],tHw:0,tIb:0,tIa:0,tOb:0,tPay:0};
    const c=calcDay(r);if(!c)return;
    byStaff[r.sid].days.push({...r,...c});
    byStaff[r.sid].tHw+=c.hw;byStaff[r.sid].tIb+=c.ib;byStaff[r.sid].tIa+=c.ia;byStaff[r.sid].tOb+=c.ob;byStaff[r.sid].tPay+=c.tp;
  });
  let h=`<div style="display:flex;gap:8px;align-items:flex-end;margin-bottom:16px;flex-wrap:wrap">
    <div class="fd"><label>年</label><select onchange="S.sy=+this.value;rs()">${yopts}</select></div>
    <div class="fd"><label>月</label><select onchange="S.sm=+this.value;rs()">${mopts}</select></div>
  </div>`;
  if(!Object.keys(byStaff).length){el.innerHTML=h+'<div class="empty">この月のデータがありません</div>';return;}

  Object.values(byStaff).forEach(s=>{
    const sid=s.id;
    h+=`<div class="card">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:14px;flex-wrap:wrap;gap:8px">
        <span style="font-size:15px;font-weight:500">${s.name}</span>
        <div style="display:flex;align-items:center;gap:10px;flex-wrap:wrap">
          <div style="text-align:right"><div style="font-size:22px;font-weight:500;color:#185FA5">¥${s.tPay.toLocaleString()}</div><div style="font-size:11px;color:var(--color-text-secondary)">${S.sy}年${S.sm}月 合計</div></div>
          <button class="btn bg" onclick="dlCSV(${sid})" style="font-size:12px;white-space:nowrap">📥 CSV保存</button>
        </div>
      </div>
      <div class="mrow">
        <div class="mt"><div class="mtl">時給合計</div><div class="mtv">¥${s.tHw.toLocaleString()}</div></div>
        <div class="mt"><div class="mtl">IC基礎額合計</div><div class="mtv" style="color:#185FA5">¥${s.tIb.toLocaleString()}</div></div>
        <div class="mt"><div class="mtl">IC加算分</div><div class="mtv" style="color:#185FA5">¥${s.tIa.toLocaleString()}</div></div>
        <div class="mt"><div class="mtl">緊急報酬</div><div class="mtv" style="color:var(--color-text-danger)">¥${s.tOb.toLocaleString()}</div></div>
      </div>
      <div style="overflow-x:auto" id="tbl-${sid}"><table>
        <thead><tr><th style="text-align:left">日付</th><th>勤務h</th><th>時給</th><th>IC基礎額</th><th>通常支給</th><th>IC加算</th><th>緊急報酬</th><th>合計支給</th></tr></thead>
        <tbody>${s.days.sort((a,b)=>a.date.localeCompare(b.date)).map(d=>`<tr>
          <td style="text-align:left;font-weight:500">${d.date}</td><td>${d.hrs}</td>
          <td>¥${d.hw.toLocaleString()}</td><td style="color:#185FA5">¥${d.ib.toLocaleString()}</td>
          <td style="color:${d.ia>0?'#185FA5':''}">¥${d.rp.toLocaleString()}</td>
          <td style="color:#185FA5">${d.ia>0?'+¥'+d.ia.toLocaleString():'-'}</td>
          <td style="color:#C0392B">${d.ob>0?'+¥'+d.ob.toLocaleString():'-'}</td>
          <td style="font-weight:500">¥${d.tp.toLocaleString()}</td>
        </tr>`).join('')}</tbody>
        <tfoot><tr>
          <td style="text-align:left">合計</td><td>-</td>
          <td>¥${s.tHw.toLocaleString()}</td><td style="color:#185FA5">¥${s.tIb.toLocaleString()}</td>
          <td>¥${(s.tPay-s.tOb).toLocaleString()}</td>
          <td style="color:#185FA5">+¥${s.tIa.toLocaleString()}</td>
          <td style="color:#C0392B">+¥${s.tOb.toLocaleString()}</td>
          <td style="font-weight:500;color:#185FA5">¥${s.tPay.toLocaleString()}</td>
        </tr></tfoot>
      </table></div>
    </div>`;
  });
  el.innerHTML=h;
  // byStaff をグローバルに保持してCSV生成で使う
  window._byStaff=byStaff;window._sy=S.sy;window._sm=S.sm;
}

function dlCSV(sid){
  const s=window._byStaff?.[sid];if(!s)return;
  const BOM='\uFEFF';
  const rows=[
    [`グロッサ訪問看護ステーション インセンティブ明細`],
    [`スタッフ`,s.name],
    [`対象期間`,`${window._sy}年${window._sm}月`],
    [],
    ['日付','勤務時間(h)','時給合計','IC基礎額','通常支給','IC加算','緊急報酬','合計支給'],
    ...s.days.sort((a,b)=>a.date.localeCompare(b.date)).map(d=>[
      d.date,d.hrs,d.hw,d.ib,d.rp,d.ia,d.ob,d.tp
    ]),
    [],
    ['合計','-',s.tHw,s.tIb,(s.tPay-s.tOb),s.tIa,s.tOb,s.tPay]
  ];
  const csv=BOM+rows.map(r=>r.map(c=>{
    const v=String(c??'');return v.includes(',')||v.includes('"')||v.includes('\n')?`"${v.replace(/"/g,'""')}"`:`${v}`;
  }).join(',')).join('\r\n');
  const blob=new Blob([csv],{type:'text/csv'});
  const url=URL.createObjectURL(blob);
  const a=document.createElement('a');
  a.href=url;a.download=`インセンティブ_${s.name}_${window._sy}年${window._sm}月.csv`;
  document.body.appendChild(a);a.click();
  setTimeout(()=>{URL.revokeObjectURL(url);document.body.removeChild(a);},1000);
  showToast(`${s.name} の明細をダウンロードしました`);
}

// ===== 設定 =====
function rset(){
  const el=document.getElementById('pane-settings');
  const stH=S.staff.length?S.staff.map(s=>`<div class="rrow"><span style="flex:1;font-weight:500">${s.name}</span><span style="color:var(--color-text-secondary)">時給 ¥${s.rate.toLocaleString()}</span><button class="btn bd" onclick="delSt(${s.id})" style="font-size:12px;padding:4px 10px">削除</button></div>`).join(''):'<div class="empty">スタッフがいません</div>';
  const feeRows=VTYPES.map(v=>{
    if(v.mode==='ojt')return`<div style="display:flex;align-items:center;gap:8px;padding:7px 0;border-bottom:0.5px solid var(--color-border-tertiary)"><span style="flex:1;font-size:13px">${v.lbl} <span style="color:var(--color-text-secondary);font-size:11px">${v.sub}</span></span><span style="font-size:12px;color:var(--color-text-secondary)">除外（変更不要）</span></div>`;
    if(v.mode==='mp')return`<div style="display:flex;align-items:center;gap:8px;padding:7px 0;border-bottom:0.5px solid var(--color-border-tertiary);flex-wrap:wrap"><span style="flex:1;min-width:120px;font-size:13px">${v.lbl.replace('\n',' ')} <span style="color:var(--color-text-secondary);font-size:11px">${v.sub}</span></span><span style="font-size:11px;color:var(--color-text-secondary)">基本¥</span><input type="number" value="${getFee(v.k)}" style="width:80px;text-align:right" oninput="S.cfg.fees['${v.k}']=+this.value"><span style="font-size:11px;color:var(--color-text-secondary)">加算¥</span><input type="number" value="${getFee(v.k,'mpA')}" style="width:80px;text-align:right" oninput="S.cfg.fees['${v.k}_mpA']=+this.value"></div>`;
    return`<div style="display:flex;align-items:center;gap:8px;padding:7px 0;border-bottom:0.5px solid var(--color-border-tertiary)"><span style="flex:1;font-size:13px">${v.lbl.replace('\n',' ')} <span style="color:var(--color-text-secondary);font-size:11px">${v.sub}</span></span><span style="font-size:11px;color:var(--color-text-secondary)">¥</span><input type="number" value="${getFee(v.k)}" style="width:90px;text-align:right" oninput="S.cfg.fees['${v.k}']=+this.value"></div>`;
  }).join('');
  el.innerHTML=`<div class="card"><div style="font-size:14px;font-weight:500;margin-bottom:12px">スタッフ管理</div>
    <div style="display:flex;gap:8px;flex-wrap:wrap;margin-bottom:10px">
      <input placeholder="氏名" value="${S.ns}" oninput="S.ns=this.value" style="width:150px">
      <input type="number" placeholder="時給（円）" value="${S.nr}" oninput="S.nr=this.value" style="width:110px">
      <button class="btn bp" onclick="addSt()">追加</button>
    </div>${stH}</div>
  <div class="card"><div style="font-size:14px;font-weight:500;margin-bottom:4px">単価・加算設定</div>
    <div style="font-size:11px;color:var(--color-text-secondary);margin-bottom:10px">診療報酬・介護報酬改定時に更新してください</div>
    ${feeRows}</div>`;
}
function addSt(){if(!S.ns||!S.nr)return;S.staff.push({id:UID++,name:S.ns,rate:+S.nr});S.ns='';S.nr='';rset();}
function delSt(id){S.staff=S.staff.filter(s=>s.id!==id);rset();}
ri();
</script>
