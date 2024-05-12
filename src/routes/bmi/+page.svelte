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

    const handleSubmit = async () => {
        const url = `https://fitness-calculator.p.rapidapi.com/bmi?age=${age}&weight=${weight}&height=${height}`;
        try {
            const response = await fetch(url, options);

            if (!response.ok) {
                if (response.status === 422) {
                    invalid = true;
                } else {
                    throw new Error('Network response was not ok.');
                }
            } else {
                bmiData = await response.json();
                invalid = false;
            }
            
        } catch (error) {
            //@ts-ignore
            console.error('Error fetching data: ', error.message);
        }    
    }

</script>

<div class=" w-screen h-screen flex flex-col items-center justify-start gap-8">
    <h1 class=" my-4 text-2xl text-blue font-medium">Calculate Your BMI</h1>
    <div class=" w-4/12 flex flex-col items-center bg-white max-h-screen overflow-y-auto justify-center gap-4
    form-shadow rounded-sm">
        <h2 class="text-lg my-2">Enter Your Data</h2>

        <div class="flex flex-col gap-2">
            <p class=" text-sm">Age: </p>
            <input bind:value={age} type="number" min="0" max="80"
            class=" outline-none border-2 h-8 px-3 rounded-sm"/>
        </div>
        
        <div class="flex flex-col gap-2" >
            <p class=" text-sm">Weight (Kg): </p>
            <input bind:value={weight} type="number" min="40" max="160"
            class=" outline-none border-2 h-8 px-3 rounded-sm"/>
        </div>
        
        <div class="flex flex-col gap-2">
            <p class=" text-sm">Height (cm): </p>
            <input bind:value={height} type="number" min="130" max="230"
            class=" outline-none border-2 h-8 px-3 rounded-sm"/>
        </div>


        <button class=" w-3/12 h-8 min-h-8 bg-blue my-4 text-white rounded-sm"
        on:click={handleSubmit}>Calculate</button>

        {#if bmiData && !invalid}
        <div class="my-6 w-1/2 flex flex-col items-center justify-center gap-2">
            <p class="">BMI: {bmiData.data.bmi}</p>
            <p class="text-ash text-sm">Healthy Range: {bmiData.data.healthy_bmi_range}</p>
            <p class={bmiData.data.health.includes('Healthy')? "healthy text-sm" : " text-red-600 text-sm"}>Status: {bmiData.data.health}</p>            
        </div>
        {/if}

        {#if invalid}
            <div class="my-4 w-5/12 flex flex-col items-center">
                <p class="text-red-600 text-sm">Please provide:  </p>
                <p class="text-red-600 text-sm">Age: 0 - 80</p>
                <p class="text-red-600 text-sm">Weight: 40 - 160</p>
                <p class="text-red-600 text-sm">Height: 130 - 230</p>
            </div>
        {/if}
    </div>
</div>

<style>
    .text-blue{
        color: #5469d4;
    }
    .bg-blue{
        background-color: #5469d4;
    }
    .form-shadow{
        box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
    }

    .text-ash{
        color: #818284;
    }

    .healthy{
        color: #2AD311;
    }
</style>