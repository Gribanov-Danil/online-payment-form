<script lang="ts">
    import lockIcon from "../assets/lock-icon.svg"
    import MaskInput from 'svelte-input-mask/MaskInput.svelte';

    import valid from "card-validator";

    let codeType = 'CVV'
    let cardNumber = ''
    let expirationDate = ''
    let mask = "0000 0000 0000 0000";


    const submitHandler = () => {


    }

    const blurHandler = () => {
        const numberValidation = valid.number(cardNumber);
        console.log(numberValidation)
    }

    const checkExpirationDate = () => {
        const expirationDateValidation = valid.expirationDate(expirationDate)
        console.log(expirationDateValidation)
    }


    const checkCodeType = (e: KeyboardEvent) => {
        if (!(/\d+/g.test(e.key))) {
            e.preventDefault()
        }
        console.log(cardNumber)
        const numberValidation = valid.number(cardNumber);
        if (numberValidation.card) {
            codeType = numberValidation.card.code.name
            console.log(codeType)
            console.log(numberValidation)
        }
        else {
            codeType = 'CVV'
        }
    }

    const handleChange = ({ detail }) => {
        const numberValidation = valid.number(detail.inputState.unmaskedValue);
        if (numberValidation.card) {
            codeType = numberValidation.card.code.name
        }
        else {
            codeType = 'CVV'
        }
    }
</script>

<div class="payment">
    <form class="payment__container" on:submit|preventDefault={submitHandler}>
        <div class="payment__header header">
            <div class="header__title title">
                <div class="title__orderAmount">Оплата: 100,00 &#8381;</div>
                <div class="title__connection">
                    <div class="tittle__icon">
                        <img src={lockIcon} alt="lockIcon">
                    </div>
                    <div class="tittle__connectionType">
                        <p>Безопасное соединение</p>
                    </div>
                </div>
            </div>
            <div class="header__description">
                <div class="payment__storeName">Магазин: DanillioStore</div>
                <div class="payment__orderNumber">Номер заказа: 00001</div>
            </div>
        </div>
        <div class="payment__body body">
            <div class="body__card card">
                <div class="card__backSide backSide">
                    <div class="backSide__container">
                        <input placeholder={codeType || "CVV"} required>
                        <a href="#">Что это?</a>

                    </div>
                </div>
                <div class="card__frontSide frontSide">
                    <div class="frontSide__container">
                        <MaskInput
                                on:change={handleChange}
                                class="test"
                                alwaysShowMask={false}
                                maskChar=""

                                placeholder="Введите номер карты"
                                {mask}
                                on:blur={blurHandler}

                                value={cardNumber}
                                required
                        />
                        <input
                                placeholder="Введите номер карты"
                                on:blur={blurHandler}
                                on:keypress={checkCodeType}
                                bind:value={cardNumber}
                                required
                        >
                        <div class="frontSide__dates">
                            <input
                                    placeholder="Месяц / Год"
                                    bind:value={expirationDate}
                                    on:blur={checkExpirationDate}
                                    required
                            >
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="payment__footer">
            <input placeholder="Введите почту для получения квитанции" required>
            <button type="submit">Оплатить</button>
        </div>
    </form>
</div>

<style>
    .payment {
        width: 570px;
        display: flex;
        align-items: center;
        border: #c3d3e4 solid 1px;
    }

    .payment__container {
        margin: 0 20px;
    }

    .payment__header {
        margin-bottom: 20px;
    }

    .header__title {
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .title__connection {
        display: flex;
        align-items: center;
    }

    .tittle__connectionType {
        max-width: 75px;
    }

    .body__card {
        position: relative;
        width: 520px;
    }

    .card__frontSide {
        height: 100%;
        position: absolute;
        top: 0;
        left: 0;
        width: 75%;
        background-color: #f2f5f6;
        border-radius: 10px;
    }

    .frontSide__container {
        padding: 40px 35px 40px 30px;
    }

    .card__backSide {
        display: flex;
        flex-direction: column;
        align-items: flex-end;
        width: 100%;
        background-color: #eaeaea;
        border-radius: 10px;
        height: 230px;
    }

    :global(.payment input) {
        height: 60px;
        padding: 0 20px;
        font-size: 16px;
        border: #c3c3c3 1px solid;
        border-radius: 5px;
    }

    .frontSide__dates {
        display: flex;
        gap: 30px;
    }

    :global(.frontSide__dates input) {
        width: 200px;
    }

    .payment input::placeholder {
        font-size: 16px;
        color: #c8cacd;
    }

    .backSide__container {
        margin-top: 120px;
        margin-right: 15px;
        display: flex;
        flex-direction: column;
    }

    .backSide__container input {
        padding: 0 10px;
        width: 75px;
    }

    .backSide__container input::placeholder {
        padding: 0;
        /*text-align: center;*/
    }

    :global(.frontSide__container > input) {
        margin-bottom: 30px;
        width: calc(100% - 40px);
    }

    .payment__body {
        margin-bottom: 80px;
    }

    .payment__footer {
        display: flex;
        justify-content: space-between;
        margin-bottom: 40px;
    }

    .payment__footer input {
        width: calc(75%);
    }

    .payment__footer button {
        border-radius: 5px;
        width: calc(25%);
    }
</style>