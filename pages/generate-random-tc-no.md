---
layout: page
title: Rastgele Tc No Üretme
order: 4
---

---

## Rastgele Tc No Üretme

<style>
  .tc-area{
    margin-top: 1rem;
  }
    .mertt-btn{
      padding: 10px 12px;
      cursor: pointer;
      border-radius: 2px;
    }
    .btn-bw {
      background-color: black;
      color: whitesmoke;
      transition: 0.3s ease-in-out;
    }
    .btn-bw:hover{
      background-color: whitesmoke;
      color: black;
    }
    .mertt-card{
      padding: 1rem;
      border: 1px solid #efefef;
    }

    #showTc, #copyBtn{
      opacity: 1;
      transition: 0.3s ease-in-out;
    }

    #showTc{
      margin : 0 1rem;
    }

    #showTc.hidden, #copyBtn.hidden {
      opacity: 0;
    }


  </style>

<div class="tc-area">
  <div class="mertt-card">
    <button class="mertt-btn btn-bw" id="generateRandomTcBtn">Rastegele TC No Üret</button>
    <span id="showTc"></span>
    <button class="mertt-btn" id="copyBtn">Kopyala</button>
  </div>
</div>

 <script>
    document.addEventListener("DOMContentLoaded", function() {
      let btn = document.getElementById("generateRandomTcBtn");
      let tcArea = document.getElementById("showTc");
      let copyBtn = document.getElementById("copyBtn");
      applyAffect(tcArea, copyBtn);
      btn.onclick = () =>{ applyAffect(tcArea, copyBtn);};
      copyBtn.onclick = () => {
        let txt = copyBtn.innerText;
        copyBtn.innerText = "Ok";
        setTimeout(() => {
          copyBtn.innerText = txt;
        }, 500);


        navigator.permissions.query({
            name: "clipboard-write"
        })
        .then(function(permissionStatus) {
            if (permissionStatus.state === "granted" || permissionStatus.state === "prompt") {
                navigator.clipboard.writeText(tcArea.innerText);
            }
        });
      };
    });

    function applyAffect(el, copyBtnEl){
      el.classList.add('hidden');
      copyBtnEl.classList.add('hidden');
      const computedStyle = window.getComputedStyle(el);
      const transitionDuration = computedStyle.transitionDuration;
      const durationInSeconds = parseFloat(transitionDuration);
      console.log(durationInSeconds);
      const durationInMilliseconds = durationInSeconds * 1000;
      setTimeout(() => {
          el.textContent = generateRandomTC();
          el.classList.remove('hidden');
          copyBtnEl.classList.remove('hidden');
      }, durationInMilliseconds);
    }

    function generateRandomTC() {
      let tcArr = [];
      let tenthDigit;
      let lastDigit;
      let oddSum = 0;
      let evenSum = 0;

      for(let i=1; i< 10; i++){
          digit = Math.floor(Math.random() * 9) + 1;
          if (i % 2 === 1) {
            oddSum += digit;
          } else {
            evenSum += digit;
          }
          tcArr.push(digit)
      }
      tenthDigit = ((oddSum * 7) - evenSum) % 10;
      tcArr.push(tenthDigit);
      lastDigit = (evenSum + oddSum + tenthDigit) % 10;
      tcArr.push(lastDigit);
      let tc = tcArr.join('');
      return tc;
    }
    console.log(generateRandomTC());
  </script>
