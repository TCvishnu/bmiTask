<script>
    let age = 0;
    let weight = 40;
    let height = 130;
    let neck = 20;
    let waist = 40;
    let hip = 40;
    let gender = "male";
    let isInvalid = false;
    //@ts-ignore
    let fatData = null;
    let open = false;
    const options = {
        method: 'GET',
        headers: {
            'X-RapidAPI-Key': '0d49f2dfd2msh804c39e5ae61c96p10ac9cjsn0ec27f1e1705',
            'X-RapidAPI-Host': 'fitness-calculator.p.rapidapi.com'
        }
    };

    const handleSubmit = () => {
        const url = `https://fitness-calculator.p.rapidapi.com/bodyfat?age=${age}&gender=${gender}&weight=${weight}&height=${height}&neck=${neck}&waist=${waist}&hip=${hip}`;
        fetch(url, options)
        .then(response => {
            if (response.ok) {
                isInvalid = false;
                return response.json();
            } else if (response.status === 422) {
                isInvalid = true
                throw new Error("Invalid request: 422");
            } else {
                throw new Error("Network response was not ok: " + response.status);
            }
        })
        .then(json => {
            fatData = json;
            open = true;
        })
        .catch(error => console.log("Error: ", error));

    }
</script>

<div class="main">
    <h1>Calculate your BodyFat</h1>
    
    <div class={(!open)? "box": "smallerBox"}>
        {#if (!open)}
            <h2>Enter your data</h2>
            <div class="formContainer">
                <div class="inputsDiv">
                    <p>Age (0-80): </p>
                    <input bind:value={age} type="number" min="0" max="80"/>

                    <p>Weight (40-160 Kg): </p>
                    <input bind:value={weight} type="number" min="40" max="160"/>

                    <p>Height (130-230 cm): </p>
                    <input bind:value={height} type="number" min="130" max="230"/>
                    
                </div>
                <div class="inputsDiv">
                    <p>Neck (20-80 cm): </p>
                    <input bind:value={neck} type="number" min="0" max="80"/>

                    <p>Waist (40-130 cm): </p>
                    <input bind:value={waist} type="number" min="40" max="130"/>

                    <p>Hip (40-130 cm): </p>
                    <input bind:value={hip} type="number" min="130" max="230"/>
                </div>
            </div>
            <div class="radioDiv">
                {#each ["male", "female"] as sex}
                    <label>
                        {`${sex[0].toLocaleUpperCase()}${sex.slice(1)}`} 
                        <input type="radio" value={sex} bind:group={gender} class="radioInp"/>
                    </label>
                {/each}
            </div>
            <button class="submitBtn" on:click={handleSubmit}>Calculate</button>

            {#if (isInvalid)}
            <p class="errorMsg">Enter appropriate details</p>
            {/if}
        {:else}
            <button class="closeBtn"
            on:click={() => {open = false}}
            title="Close">x</button>
            
            <div class="fat-info">
                <h2 class="infoh2">Your body fat is:</h2>
                <p class="infoP">
                    <span class="fat-type">Body fat (BMI Method):</span> {fatData.data["Body Fat (BMI method)"]}
                </p>
                <p class="infoP">
                    <span class="fat-type">Body fat (US Navy Method):</span> {fatData.data["Body Fat (U.S. Navy Method)"]}
                </p>
                <p class="infoP">
                    <span class="fat-type">Body fat mass:</span> {fatData.data["Body Fat Mass"]}
                </p>
                <p class="infoP">
                    <span class="fat-type">Lean Body Mass:</span> {fatData.data["Lean Body Mass"]}
                </p>
            </div>

        {/if}
    </div>
    
</div>


<style>
    h1 {
        color: #5469d4;
    }

    h2 {
        font-size: large;
        text-align: center;
    }

    .main{
        width: 100%;
        height: 100vh;
        display: flex;
        flex-direction: column;
        justify-content: start;
        align-items: center;
        overflow-y: auto;
    }
    .box {
        width: 50%;
        height: 75%;
        background-color: white;
        box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
        border-radius: 2px;
        align-items: center;
        display: flex;
        flex-direction: column;
        overflow-y: auto;
        gap: 2%;
    }
    .closeBtn{
        position: absolute;
        left: 0;
        top: 0;
        background-color: red;
        border: none;
        width: 4.5%;
        height: 6%;
        color: #E8E9EB;
        border-radius: 1px;
    }
    .smallerBox{
        width: 40%;
        height: 55%;
        background-color: white;
        box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
        border-radius: 2px;
        align-items: center;
        display: flex;
        flex-direction: column;
        justify-content: center;
        overflow-y: auto;
        gap: 2%;
        position: relative;
    }
    .formContainer {
        width: 100%;
        height: 50%;
        display: flex;
        flex-direction: row;
        align-items: center;
        overflow-y: auto;
    }
    .inputsDiv {
        width: 50%;
        height: 100%;
        gap: 2%;
        display: flex;
        flex-direction: column;
        align-items: center;
        overflow-y: auto;
    }
    .inputsDiv p{
        font-size: small;
    }

    
    input {
        width: 70%;
        outline: none;
        padding-left: 3%;
        border: 1.5px solid #E8E9EB;
        border-radius: 5px;
        min-height: 25px;
        font-size: small;
    }

    .submitBtn{
        margin-top: 5%;
        height: 7%;
        width: 35%;
        padding-left: 0%;
        background-color: #5469d4;
        color: white;
        border: none;
        min-height: 30px;
        border-radius: 5px;
    }

    .radioDiv{
        width: 100%;
        height: 5%;
        display: flex;
        flex-direction: row;
        justify-content: space-around;
        align-items: center;
        height: 8%;
    }
    .radioDiv label {
        width: 20%;
        height: 100%;
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        font-size: small;
    }

    .fat-info {
    font-family: Arial, sans-serif;
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
  }

  .infoh2 {
    font-size: 24px;
    color: #333;
    text-align: center;
    margin-bottom: 20px;
  }

  .infoP {
    font-size: 16px;
    color: #666;
    margin-bottom: 10px;
  }

  .fat-type {
    font-weight: bold;
    color: #333;
  }

  .errorMsg{
    font-size: medium;
    text-align: center;
    color: #f32013;
  }
</style>