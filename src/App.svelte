<script>  
  let prescriptions = $state([]) 
  let modal_form;
  
  $effect(() => {
    const storePrescriptions = localStorage.getItem("prescriptionsCalculatorData")
    if (storePrescriptions) prescriptions = JSON.parse(storePrescriptions)
  })
 
  $effect(() => {
    localStorage.setItem("prescriptionsCalculatorData", JSON.stringify(prescriptions))
  })
 

  const decrementQty = (idx) => {   
    const prescription = prescriptions[idx] 
    if (prescription.quantity < 1) return
    prescription.quantity = Number(prescription.quantity) - 1
  }
  const incrementQty = (idx) => {   
    const prescription = prescriptions[idx] 
    prescription.quantity = Number(prescription.quantity) + 1
  }
  const updateQty = (e, idx) => {
    const prescription = prescriptions[idx]
    prescription.quantity = Number(e.target.value)  
  }
  const updatePrice = (e, idx) => {
    const prescription = prescriptions[idx]
    prescription.price = Number(e.target.value) 
  }
 
  
  const add = (e) => { 
    e.preventDefault()
    const formData = new FormData(e.target) 
    const data = Object.fromEntries(formData) 
    // @ts-ignore
    prescriptions.push(data) 
    e.target.reset()
    modal_form.close()
  } 
</script>
 
 
<!-- <button class="btn" onclick={() => modal_form.showModal()}>open modal</button> -->

 
<dialog bind:this={modal_form} id="my_modal_form" class="modal modal-bottom sm:modal-middle">
  <div class="modal-box">
    <form method="dialog">
      <button class="btn btn-sm btn-circle btn-ghost absolute right-2 top-2">✕</button>
    </form>
    <form onsubmit={add} class="mx-2" action="">
      <h1 class="text-center font-bold text-lg">Add Prescription</h1>
      <label  class="form-control w-full">
        <div class="label">
            <span class="label-text">Name</span> 
        </div>
        <input placeholder="e.g. Paracetamol 200mg" class='input input-sm input-bordered w-full' type="text" name="name" id=""> 
      </label>
      <div class="flex gap-4">
        <label  class="form-control w-full">
          <div class="label">
              <span class="label-text">Quantity</span> 
          </div>
          <input placeholder="e.g. 21" class='input input-sm input-bordered w-full' type="number" name="quantity" step="any" id=""> 
        </label>
        <label  class="form-control w-full">
          <div class="label">
              <span class="label-text">Unit</span> 
          </div>
          <input placeholder="e.g. tablet, capsule" class='input input-sm input-bordered w-full' type="text" name="unit" id=""> 
        </label>  
        <label  class="form-control w-full">
          <div class="label">
              <span class="label-text">Est. price (₱)</span> 
          </div>
          <input placeholder="e.g. 20.50" class='input input-sm input-bordered w-full' type="number" step="any" name="price" id=""> 
        </label>  
      </div>
      <label  class="form-control w-full mb-4">
        <div class="label">
            <span class="label-text">Frequency</span> 
        </div>
        <div class="flex align-middle items-center">
          <input placeholder="e.g. 3" class='input input-sm input-bordered w-full' type="number" min="0" name="dosage" id="">
          <h1 class="mx-2">per</h1>
          <select name="period" class="select select-sm select-bordered w-full max-w-xs">
            <option selected>Day</option>
            <option>Week</option>
            <option>Month</option>
          </select>
        </div>
      </label>
      <button type="submit" class="btn btn-primary w-full">Add</button>
    </form> 
  </div>
</dialog>

<main class="mx-0 lg:mx-auto h-screen"> 

  <div class="flex mt-8 items-center justify-center">  
    <div class="font-sans w-full mx-4 lg:mx-0 max-w-4xl">
      <h1 class="text-2xl font-extrabold text-gray-800">Prescription Calculator</h1>
      <div class="grid md:grid-cols-3 gap-4 mt-4">
          <div class="md:col-span-2 space-y-4"> 

            {#each prescriptions as prescription, idx (idx)}
              <div class="flex gap-4 bg-white px-3 py-4 rounded-md shadow-[0_2px_12px_-3px_rgba(6,81,237,0.3)]">
                <div class="flex gap-4"> 
                    <div class="flex  flex-col gap-3">
                        <div>
                          <div class="mt-2 gap-1 flex align-baseline">
                            <div>
                              <div class="dropdown">
                                <div tabindex="0" role="button" class="btn btn-xs btn-primary btn-circle m-1">
                                  <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24">
                                    <path fill="currentColor" d="m9.25 22l-.4-3.2q-.325-.125-.612-.3t-.563-.375L4.7 19.375l-2.75-4.75l2.575-1.95Q4.5 12.5 4.5 12.338v-.675q0-.163.025-.338L1.95 9.375l2.75-4.75l2.975 1.25q.275-.2.575-.375t.6-.3l.4-3.2h5.5l.4 3.2q.325.125.613.3t.562.375l2.975-1.25l2.75 4.75l-2.575 1.95q.025.175.025.338v.674q0 .163-.05.338l2.575 1.95l-2.75 4.75l-2.95-1.25q-.275.2-.575.375t-.6.3l-.4 3.2zm2.8-6.5q1.45 0 2.475-1.025T15.55 12t-1.025-2.475T12.05 8.5q-1.475 0-2.488 1.025T8.55 12t1.013 2.475T12.05 15.5" />
                                  </svg>
                                </div>
                                <!-- svelte-ignore a11y_no_noninteractive_tabindex -->
                                <ul tabindex="0" class="dropdown-content menu bg-base-100 rounded-box z-[1] w-52 p-2 shadow">
                                  <li>
                                    <button class="btn btn-sm">
                                      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24">
                                        <path fill="currentColor" d="M19 4h-3.5l-1-1h-5l-1 1H5v2h14M6 19a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V7H6z" />
                                      </svg>
                                      Delete
                                    </button>
                                  </li>
                                  <li>
                                    <button class="btn btn-sm"> 
                                      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24">
                                        <path fill="currentColor" d="M3 21v-4.25L16.2 3.575q.3-.275.663-.425t.762-.15t.775.15t.65.45L20.425 5q.3.275.438.65T21 6.4q0 .4-.137.763t-.438.662L7.25 21zM17.6 7.8L19 6.4L17.6 5l-1.4 1.4z" />
                                      </svg>
                                      Edit
                                    </button>
                                  </li>
                                  <li>
                                    <button class="btn btn-sm">
                                      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24">
                                        <path fill="currentColor" d="M11.83 9L15 12.16V12a3 3 0 0 0-3-3zm-4.3.8l1.55 1.55c-.05.21-.08.42-.08.65a3 3 0 0 0 3 3c.22 0 .44-.03.65-.08l1.55 1.55c-.67.33-1.41.53-2.2.53a5 5 0 0 1-5-5c0-.79.2-1.53.53-2.2M2 4.27l2.28 2.28l.45.45C3.08 8.3 1.78 10 1 12c1.73 4.39 6 7.5 11 7.5c1.55 0 3.03-.3 4.38-.84l.43.42L19.73 22L21 20.73L3.27 3M12 7a5 5 0 0 1 5 5c0 .64-.13 1.26-.36 1.82l2.93 2.93c1.5-1.25 2.7-2.89 3.43-4.75c-1.73-4.39-6-7.5-11-7.5c-1.4 0-2.74.25-4 .7l2.17 2.15C10.74 7.13 11.35 7 12 7" />
                                      </svg>
                                      Hide
                                    </button>
                                  </li> 
                                </ul>
                              </div>
                              <!-- svelte-ignore a11y_consider_explicit_label -->
                              <!-- <button class="btn btn-xs btn-primary btn-circle">
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24">
                                  <path fill="currentColor" d="m9.25 22l-.4-3.2q-.325-.125-.612-.3t-.563-.375L4.7 19.375l-2.75-4.75l2.575-1.95Q4.5 12.5 4.5 12.338v-.675q0-.163.025-.338L1.95 9.375l2.75-4.75l2.975 1.25q.275-.2.575-.375t.6-.3l.4-3.2h5.5l.4 3.2q.325.125.613.3t.562.375l2.975-1.25l2.75 4.75l-2.575 1.95q.025.175.025.338v.674q0 .163-.05.338l2.575 1.95l-2.75 4.75l-2.95-1.25q-.275.2-.575.375t-.6.3l-.4 3.2zm2.8-6.5q1.45 0 2.475-1.025T15.55 12t-1.025-2.475T12.05 8.5q-1.475 0-2.488 1.025T8.55 12t1.013 2.475T12.05 15.5" />
                                </svg>
                              </button> -->
                            </div>
                            <h3 class="text-base font-bold text-gray-800 line-clamp-1"> {prescription.name}</h3>
                          </div>
                          <p class="text-xs lg:text-sm font-semibold text-gray-500 mt-2 flex items-center gap-2">{prescription.quantity} {prescription.unit}s | {prescription.dosage}x per {prescription.period}</p> 
                        </div>

                        <div class="mt-auto flex items-center gap-3"> 
                          <!-- svelte-ignore a11y_consider_explicit_label -->
                          <button onclick={() => decrementQty(idx)} class="btn btn-primary btn-xs btn-circle">
                            <svg xmlns="http://www.w3.org/2000/svg" class="w-2 fill-white" viewBox="0 0 124 124">
                              <path d="M112 50H12C5.4 50 0 55.4 0 62s5.4 12 12 12h100c6.6 0 12-5.4 12-12s-5.4-12-12-12z" data-original="#000000"></path>
                            </svg>
                          </button>
                           
                          <input oninput={(e) => updateQty(e, idx)} type="number" min="0" class="input input-sm w-16 input-bordered  max-w-xs" value={prescription.quantity} />
                          <!-- svelte-ignore a11y_consider_explicit_label -->
                          <button onclick={() => incrementQty(idx)} class="btn btn-primary btn-xs btn-circle">
                              <svg xmlns="http://www.w3.org/2000/svg" class="w-2 fill-white" viewBox="0 0 42 42">
                                  <path d="M37.059 16H26V4.941C26 2.224 23.718 0 21 0s-5 2.224-5 4.941V16H4.941C2.224 16 0 18.282 0 21s2.224 5 4.941 5H16v11.059C16 39.776 18.282 42 21 42s5-2.224 5-4.941V26h11.059C39.776 26 42 23.718 42 21s-2.224-5-4.941-5z" data-original="#000000"></path>
                              </svg>
                          </button>
                        </div>
                    </div>
                </div>

                <div class="ml-auto flex flex-col justify-end">
                    <div class="flex items-start gap-4 justify-end"> 
                      <label class="input input-bordered flex input-sm items-center gap-2">
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 20 20">
                          <g fill="currentColor" fill-rule="evenodd" clip-rule="evenodd">
                            <path d="M11 3.5H6v-2h5a5 5 0 0 1 5 5v1a5 5 0 0 1-5 5H6v-2h5a3 3 0 0 0 3-3v-1a3 3 0 0 0-3-3" />
                            <path d="M6 1.5a1 1 0 0 1 1 1V18a1 1 0 1 1-2 0V2.5a1 1 0 0 1 1-1" />
                            <path d="M2 5.436a1 1 0 0 1 1-1h14a1 1 0 1 1 0 2H3a1 1 0 0 1-1-1m0 3a1 1 0 0 1 1-1h14a1 1 0 1 1 0 2H3a1 1 0 0 1-1-1" />
                          </g>
                        </svg>
                        <input oninput={(e) => updatePrice(e, idx)} min="0" type="number" class="grow w-8 lg:w-20" placeholder="price" value={prescription.price} />
                        <span>each</span>
                      </label>
                    </div> 
                    <h6 class="text-right text-xs lg:text-sm font-semibold text-gray-500 mt-2">will last {Math.floor(prescription.quantity / (prescription.dosage))} {prescription.quantity / (prescription.dosage) > 1 ? `${prescription.period}s` : `${prescription.period}`}</h6> 
                    <h3 class="text-right text-base font-bold text-gray-800 mt-auto">₱{(prescription.price * prescription.quantity).toLocaleString()}</h3>
                </div>

              </div> 

            {:else}
            <h1 class="text-center font-bold text-lg">No prescriptions</h1>
            {/each}
           
            <!-- svelte-ignore a11y_consider_explicit_label -->
            <button onclick={()=>modal_form.showModal()} type="button" class="btn btn-primary w-full">
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24">
                <path fill="currentColor" d="M19 12.998h-6v6h-2v-6H5v-2h6v-6h2v6h6z" />
              </svg>
              Add
            </button>
          </div> 
 
          <div class="bg-white rounded-md px-4 py-6 h-max shadow-[0_2px_12px_-3px_rgba(6,81,237,0.3)]">
              <ul class="text-gray-800 space-y-4">
                  <li class="flex flex-wrap gap-4 text-sm">Subtotal <span class="ml-auto font-bold">$200.00</span></li>
                  <li class="flex flex-wrap gap-4 text-sm">Shipping <span class="ml-auto font-bold">$2.00</span></li>
                  <li class="flex flex-wrap gap-4 text-sm">Tax <span class="ml-auto font-bold">$4.00</span></li>
                  <hr class="border-gray-300" />
                  <li class="flex flex-wrap gap-4 text-sm font-bold">Total <span class="ml-auto">$206.00</span></li>
              </ul>

              <div class="mt-8 space-y-2">
                  <button type="button" class="text-sm px-4 py-2.5 w-full font-semibold tracking-wide bg-gray-800 hover:bg-gray-900 text-white rounded-md">Buy Now</button>
                  <button type="button" class="text-sm px-4 py-2.5 w-full font-semibold tracking-wide bg-transparent hover:bg-gray-100 text-gray-800 border border-gray-300 rounded-md">Continue Shopping  </button>
              </div>

              <div class="mt-4 flex flex-wrap justify-center gap-4">
                  <img src='https://readymadeui.com/images/master.webp' alt="card1" class="w-10 object-contain" />
                  <img src='https://readymadeui.com/images/visa.webp' alt="card2" class="w-10 object-contain" />
                  <img src='https://readymadeui.com/images/american-express.webp' alt="card3" class="w-10 object-contain" />
              </div>
          </div>
      </div>
  </div>
  </div> 


</main>


