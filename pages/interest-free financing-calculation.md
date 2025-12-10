---
layout: page
title: Evim Finansman Hesaplama
order: 4
---

<div>
  <label for="organizationFeeRate">Organizasyon Ücreti Oranı (%)</label>
  <input id="organizationFeeRate" type="text" />
  <br />

<label for="monthlyInstallmentFee">Aylık Taksit Tutarı: </span>
<input id="monthlyInstallmentFee" type="text" />
<br />
<label for="loan">Kredi</label>
<input id="loan" type="text" />

<br />
<button  id="calc">Hesapla</button>

  <hr />
  <br />
  <span>Organizasyon Ücreti: </span>
  <div id="organizationFee"></div>
  <br />
  <span>Aylık Taksit Sayısı: </span>
  <div id="monthlyInstallmentCount"></div>
    <br />
  <span>Paranın Alınacağı Ay: </span>
  <div id="inWhichMonth"></div>
</div>

<script>
      const orgFee = document.getElementById("organizationFee");
      const orgFeeRate = document.getElementById("organizationFeeRate");
      const loan = document.getElementById("loan");

      document.getElementById("calc").addEventListener("click", () => {
        const organizationFeeRate = parseFloat(orgFeeRate.value) / 100;
        const loanAmount = parseFloat(loan.value);

        if (isNaN(organizationFeeRate) || isNaN(loanAmount)) {
          document.getElementById("organizationFee").innerText =
            "Lütfen geçerli sayılar giriniz.";
          return;
        }

        const organizationFee = loanAmount * organizationFeeRate;

        document.getElementById(
          "organizationFee"
        ).innerText = `Organizasyon Ücreti: ${organizationFee.toFixed(2)}`;

        const monthlyInstallmentFee = parseFloat(
          document.getElementById("monthlyInstallmentFee").value
        );
        if (isNaN(monthlyInstallmentFee) || monthlyInstallmentFee <= 0) {
          document.getElementById("monthlyInstallmentFee").innerText =
            "Lütfen geçerli aylık taksit tutarı giriniz.";
          return;
        }

        const installmentCount = Math.ceil(loanAmount / monthlyInstallmentFee);

        document.getElementById(
          "monthlyInstallmentCount"
        ).innerText = `Aylık Taksit Sayısı: ${installmentCount}`;

        const inWhichMonth =  ((loanAmount * 0.4) / monthlyInstallmentFee).toFixed(0);
        console.log((loanAmount * 0.4) / monthlyInstallmentFee);
        document.getElementById(
          "inWhichMonth"
        ).innerText = inWhichMonth < 5 ? 5 : inWhichMonth ; // en hizli 5.ayda alinir
      });
    </script>
