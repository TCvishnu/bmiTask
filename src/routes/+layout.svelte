<slot />
<div>
    <nav>
        <a href="/bmi" class="end">BMI </a>   
        <a href="/bodymass" class="start">BodyMass</a>
    </nav>
</div>

<style>
    div{
        position: fixed;
        bottom: 1%;
        width: 100%;
        height: 5%;
        display: flex;
        justify-content: center;
        align-items: center;
        overflow-y: auto;
    }

    nav {
        width: 30%;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 5%;
        background-color: #5469d4;
        border-radius: 4px;
        position: relative;
    }

    a {
        width: 40%;
        text-decoration: none;
        color: #313638;
        display: flex;
        justify-content: center;
        align-items: center;
        color: white;
    }
    
</style>

<script>
    import "../app.css";
</script>