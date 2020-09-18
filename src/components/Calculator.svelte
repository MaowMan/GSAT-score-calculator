<script>
  import { Input, Field, Snackbar, Button, Icon } from "svelma";
  import { slide } from "svelte/transition";
  function reset() {
    score = 0;
    gap = 0;
  }
  function calculate() {
    if (score == 0 || gap == 0) {
      return;
    }
    if (score < 0 || gap < 0) {
      reset();
      Snackbar.create({ type: "is-danger", message: "輸入錯誤" });
      return;
    }
    let local_score = parseFloat(score);
    let local_gap = parseFloat(gap);
    ready = false;
    result = 1;
    while (result < 15) {
      //console.log({score:local_score , gap:local_gap});
      if (local_score >= local_gap) {
        result += 1;
        local_score -= local_gap;
      } else {
        break;
      }
    }
    //console.log({result:result});
    ready = true;
  }
  let score = 0;
  let gap = 0;
  let result = 1;
  let ready = false;
</script>

<section class="section">
  <div class="columns is-centered">
    <div class="column is-one-quarter">
      <Field label="輸入原始分數：">
        <Input type="number" icon="user-graduate" bind:value={score} />
      </Field>
    </div>
    <div class="column is-one-quarter">
      <Field label="輸入級距：">
        <Input type="number" icon="wave-square" bind:value={gap} />
      </Field>
    </div>
  </div>
  <div class="columns is-centered">
    <div class="column is-half">
      <div class="buttons">
        <Button
          type="is-link"
          iconLeft="laptop-code"
          on:click={() => calculate()}>
          計算級分
        </Button>
        {#if ready}
          <div class="container" transition:slide>
            <Button type="is-success" outlined>
              級分：{result}級{#if result == 15}好電{/if}
            </Button>
          </div>
        {/if}
      </div>
    </div>
  </div>
</section>
