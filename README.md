
<section class="panel">
  <h2>🍩 Chi tiêu theo danh mục</h2>
  <div style="max-width:360px;margin:auto">
    <canvas id="chart" width="360" height="300"></canvas>
  </div>
  <div id="legend"></div>
</section>

<script>
function drawChart(){
 const cats=[...new Set(
  data.filter(x=>x.type==='expense').map(x=>x.category)
 )];
 const vals=cats.map(cat=>data
  .filter(x=>x.category===cat&&x.type==='expense')
  .reduce((s,x)=>s+x.money,0));
 const colors=['#5278ef','#ff9f43','#36b37e',
 '#ed6482','#9b70df','#38b9c9'];
 const canvas=document.getElementById('chart');
 const ctx=canvas.getContext('2d');
 ctx.clearRect(0,0,360,300);
 const total=vals.reduce((a,b)=>a+b,0);
 let angle=-Math.PI/2;
 if(total===0){
  ctx.fillStyle='#dce2ed';
  ctx.beginPath();ctx.arc(180,140,100,0,Math.PI*2);
  ctx.fill();
 }else{
  vals.forEach((v,i)=>{
   const next=angle+v/total*Math.PI*2;
   ctx.beginPath();
   ctx.arc(180,140,100,angle,next);
   ctx.arc(180,140,57,next,angle,true);
   ctx.closePath();
   ctx.fillStyle=colors[i%colors.length];
   ctx.fill();
   angle=next;
  });
 }
 ctx.textAlign='center';
 ctx.fillStyle='#202b42';
 ctx.font='bold 17px Arial';
 ctx.fillText('Tổng chi',180,132);
 ctx.font='bold 16px Arial';
 ctx.fillText(total.toLocaleString('vi-VN')+' ₫',180,157);
 document.getElementById('legend').innerHTML=
 cats.map((cat,i)=>`
 <div style="display:flex;justify-content:space-between;
 padding:9px;border-bottom:1px solid #eee">
 <span>${colors[i%colors.length]?'●':''} ${cat}</span>
 <b>${(vals[i]/(total||1)*100).toFixed(1)}%</b>
 </div>`).join('');
}
</script>
