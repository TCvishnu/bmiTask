<script>
    let age = 0;
    let weight = 40;
    let height = 130;
    //@ts-ignore
    let bmiData = null;
    let invalid = false;
    
    const options = {
        method: 'GET',
        headers: {
            'X-RapidAPI-Key': '0d49f2dfd2msh804c39e5ae61c96p10ac9cjsn0ec27f1e1705',
            'X-RapidAPI-Host': 'fitness-calculator.p.rapidapi.com'
        }
    };

    const handleSubmit = () => {
        const url = `https://fitness-calculator.p.rapidapi.com/bmi?age=${age}&weight=${weight}&height=${height}`;
        fetch(url, options)
        .then(response => {
        if (response.ok) {
            invalid = false; // Correct data set provided
            return response.json();
        } else if (response.status === 422) {
            invalid = true //Display required input condition for BMI Calculation
        } else {
            throw new Error("Network response was not ok: " + response.status);
        }
    })
    .then(json => {
        bmiData = json;
    })
    .catch(error => console.log("Error: ", error));

    }

</script>



<div class="main">
    <h1>Calculate Your BMI</h1>
    <div class="box">
        <h2>Enter Your Data</h2>

        <p>Age: </p>
        <input bind:value={age} type="number" min="0" max="80"/>

        <p>Weight (Kg): </p>
        <input bind:value={weight} type="number" min="40" max="160"/>

        <p>Height (cm): </p>
        <input bind:value={height} type="number" min="130" max="230"/>

        <button class="submitBtn"
        on:click={handleSubmit}>Calculate</button>

        {#if bmiData && !invalid}
        <div class="statusDiv">
            <p class="bmiP">BMI: {bmiData.data.bmi}</p>
            <p class="rangeP">Healthy Range: {bmiData.data.healthy_bmi_range}</p>
            <p class={bmiData.data.health.includes('Healthy')? "healthy" : "risky"}>Status: {bmiData.data.health}</p>            
        </div>
        {/if}

        {#if invalid}
            <div class="invalidDiv">
                <p class="risky small">Please provide:  </p>
                <p class="risky small">Age: 0 - 80</p>
                <p class="risky small">Weight: 40 - 160</p>
                <p class="risky small">Height: 130 - 230</p>
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
        width: 35%;
        height: 75%;
        background-color: white;
        box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
        border-radius: 2px;
        align-items: center;
        display: flex;
        flex-direction: column;
        overflow-y: auto;
    }

    input {
        width: 60%;
        height: 6%;
        outline: none;
        padding-left: 3%;
        border: 1.5px solid #E8E9EB;
        border-radius: 5px;
        min-height: 25px;
        font-size: small;
    }
    p {
        width: 60%;
        text-align: start;
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

    .statusDiv {
        width: 80%;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        margin-top: 4%;
    }

    .bmiP {
        font-size: medium;
        text-align: center;
    }

    .rangeP{
        font-size: small;
        text-align: center;
        color: #818284;
    }

    .healthy{
        font-size: medium;
        text-align: center;
        color: #2AD311;
    }

    .risky {
        font-size: medium;
        text-align: center;
        color: #f32013;
    }
    .small {
        font-size: small;
        height: 1%;
    }

    .invalidDiv {
        width: 70%;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
</style>