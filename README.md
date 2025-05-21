# Dose-calculator-by-Blessing-Tanga-
Medicine
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Blessing Tanga's Diary - Dose Calculator</title>
<style>
  body { font-family: Arial, sans-serif; margin: 20px; max-width: 600px; }
  h1 { text-align: center; color: #2a7ae2; }
  label { display: block; margin-top: 15px; }
  input, select { width: 100%; padding: 8px; margin-top: 5px; }
  button { margin-top: 20px; padding: 10px; background: #2a7ae2; color: white; border: none; cursor: pointer; width: 100%; font-size: 16px; }
  button:hover { background: #1454a6; }
  #result { margin-top: 30px; padding: 15px; background: #f0f8ff; border-radius: 6px; }
</style>
</head>
<body>

<h1>Blessing Tanga's Diary</h1>

<form id="doseForm">
  <label for="diagnosis">Select Diagnosis:</label>
  <select id="diagnosis" required>
    <option value="" disabled selected>Select a diagnosis</option>
    <option value="pneumonia">Pneumonia</option>
    <option value="uti">Urinary Tract Infection (UTI)</option>
    <option value="hypertension">Hypertension</option>
    <option value="asthma">Asthma (acute)</option>
    <option value="diabetes">Diabetes Mellitus Type 2</option>
  </select>

  <label for="weight">Patient Weight (kg):</label>
  <input type="number" id="weight" min="1" step="0.1" required />

  <label for="age">Patient Age (years):</label>
  <input type="number" id="age" min="0" step="1" required />

  <button type="submit">Calculate Dose</button>
</form>

<div id="result"></div>

<script>
  const medsDatabase = {
    pneumonia: {
      drug: "Amoxicillin",
      dosing: (weight, age) => {
        if(age >= 18) {
          return { dose: "500 mg", frequency: "TID", route: "Oral", duration: "5-7 days" };
        } else {
          const totalDaily = 40 * weight; 
          const dosePerTime = Math.round(totalDaily / 3);
          return { dose: dosePerTime + " mg", frequency: "TID", route: "Oral", duration: "5-7 days" };
        }
      }
    },
    uti: {
      drug: "Nitrofurantoin",
      dosing: (weight, age) => {
        if(age >= 18) {
          return { dose: "100 mg", frequency: "BID", route: "Oral", duration: "5 days" };
        } else {
          const totalDaily = 6 * weight;
          const dosePerTime = Math.round(totalDaily / 2);
          return { dose: dosePerTime + " mg", frequency: "BID", route: "Oral", duration: "5 days" };
        }
      }
    },
    hypertension: {
      drug: "Amlodipine",
      dosing: (weight, age) => {
        if(age >= 18) {
          return { dose: "5-10 mg", frequency: "Once daily", route: "Oral", duration: "Chronic" };
        } else {
          return { dose: "Not recommended for children", frequency: "-", route: "-", duration: "-" };
        }
      }
    },
    asthma: {
      drug: "Salbutamol",
      dosing: (weight, age) => {
        return { dose: "2.5 mg", frequency: "q6h", route: "Nebulized", duration: "As needed" };
      }
    },
    diabetes: {
      drug: "Metformin",
      dosing: (weight, age) => {
        if(age >= 18) {
          return { dose: "500 mg", frequency: "BID", route: "Oral", duration: "Chronic" };
        } else {
          return { dose: "Not usually recommended for children", frequency: "-", route: "-", duration: "-" };
        }
      }
    }
  };

  document.getElementById("doseForm").addEventListener("submit", function(e) {
    e.preventDefault();

    const diagnosis = document.getElementById("diagnosis").value;
    const weight = parseFloat(document.getElementById("weight").value);
    const age = parseInt(document.getElementById("age").value);

    if(!diagnosis || !weight || isNaN(age)) {
      alert("Please fill in all fields correctly.");
      return;
    }

    const med = medsDatabase[diagnosis];
    if(!med) {
      document.getElementById("result").innerHTML = "<p>No medication data available for selected diagnosis.</p>";
      return;
    }

    const doseInfo = med.dosing(weight, age);

    document.getElementById("result").innerHTML = `
      <h2>Recommended Medication</h2>
      <p><strong>Diagnosis:</strong> ${diagnosis.charAt(0).toUpperCase() + diagnosis.slice(1)}</p>
      <p><strong>Drug:</strong> ${med.drug}</p>
      <p><strong>Dose:</strong> ${doseInfo.dose}</p>
      <p><strong>Frequency:</strong> ${doseInfo.frequency}</p>
      <p><strong>Route:</strong> ${doseInfo.route}</p>
      <p><strong>Duration:</strong> ${doseInfo.duration}</p>
    `;
  });
</script>

</body>
</html>
