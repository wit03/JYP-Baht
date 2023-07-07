<script>
    let withVat = true;
    let yen = 0;
    let rate = 0;

    fetch("https://open.er-api.com/v6/latest/JPY", {
        method: "GET",
    })
        .then((res) => res.json())
        .then((res) => {
            rate = res.rates.THB.toFixed(4);
        }).catch((err) => {
            console.log(err, "using fallback rate");
            rate = 0.25;
        });
</script>

<div class="flex flex-col justify-center items-center h-screen gap-5 mx-[10vw]">
    <div class="flex gap-2">
        {#if withVat}
            Include VAT
        {:else}
            Exclude VAT
        {/if}
        <input type="checkbox" class="toggle" bind:checked={withVat} />
    </div>

    <input
        type="text"
        placeholder="JPY"
        class="input input-bordered input-primary w-full max-w-xs text-xl"
        bind:value={yen}
    />

    <div class="flex">
        <div>Price in THB:</div>
        <input
            type="text"
            placeholder="Bath"
            class="input input-bordered input-info w-full max-w-xs"
            value={withVat && yen >= 5000
                ? ((yen - yen * 0.08) * rate).toFixed(2)
                : (yen * rate).toFixed(2)}
            disabled
        />
    </div>
    <p class="self-start text-sm">1 JPY = {rate} THB</p>

    <!-- svelte-ignore a11y-no-noninteractive-tabindex -->
    <div
        tabindex="0"
        class="collapse collapse-arrow border border-base-300 bg-base-200"
    >
        <div class="collapse-title font-medium">Japan Tax Refund Policy</div>
        <div class="collapse-content flex flex-col gap-5">
            <div class="flex flex-col gap-1">
                <b>ยา/เครื่องสำอาง/อาหาร</b>
                <ul class="text-xs list-inside list-disc">
                    <li>ราคาระหว่าง 5,000 ถึง 500,000 เยน</li>
                    <li>
                        ค่าภาษีที่คืน 8% ของราคาสินค้า (รวมภาษีมูลค่าเพิ่มแล้ว)
                    </li>
                </ul>
            </div>
            <div class="flex flex-col gap-1">
                <b>เครื่องใช้ไฟฟ้า/เสื้อผ้า/อื่นๆ</b>
                <ul class="text-xs list-inside list-disc">
                    <li>ราคามากกว่า 5,000 เยน</li>
                    <li>
                        ค่าภาษีที่คืน 8% ของราคาสินค้า (รวมภาษีมูลค่าเพิ่มแล้ว)
                    </li>
                </ul>
            </div>
        </div>
    </div>
</div>
