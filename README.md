# Turn-server
Test Turn Server 
```html
<html>
<head>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/js/bootstrap.min.js"></script>
</head>
<body>
<div class="container">
  <form id="idForm">
    <h2>Turn Server test</h2>
    <div class="row">
      <div class="col-md-6">
        <div class="form-group">
          <label for="first">Url</label>
          <input type="text" class="form-control" placeholder="" id="url" required>
        </div>
      </div>
      <!--  col-md-6   -->

      <div class="col-md-6">
        <div class="form-group">
          <label for="last">username</label>
          <input type="text" class="form-control" placeholder="" id="username" required >
        </div>
      </div>
      <!--  col-md-6   -->
    </div>


    <div class="row">
      <div class="col-md-6">
        <div class="form-group">
          <label for="company">password</label>
          <input type="text" class="form-control" placeholder="" id="password" required>
        </div>


      </div>

    </div>
    <!--  row   -->




    <button type="submit" class="btn btn-primary">Test</button>
  </form>
  <div class="col-md-6">
        <div class="form-group">
          <label for="company">RESULT</label>
<span id="result"></span>
          
        </div>


      </div>
</div>
<script>
function checkTURNServer(turnConfig, timeout){ 

  return new Promise(function(resolve, reject){

    setTimeout(function(){
        if(promiseResolved) return;
        resolve(false);
        promiseResolved = true;
    }, timeout || 5000);

    var promiseResolved = false
      , myPeerConnection = window.RTCPeerConnection || window.mozRTCPeerConnection || window.webkitRTCPeerConnection   //compatibility for firefox and chrome
      , pc = new myPeerConnection({iceServers:[turnConfig]})
      , noop = function(){};
    pc.createDataChannel("");    //create a bogus data channel
    pc.createOffer(function(sdp){
      if(sdp.sdp.indexOf('typ relay') > -1){ // sometimes sdp contains the ice candidates...
        promiseResolved = true;
        resolve(true);
      }
      pc.setLocalDescription(sdp, noop, noop);
    }, noop);    // create offer and set local description
    pc.onicecandidate = function(ice){  //listen for candidate events
      if(promiseResolved || !ice || !ice.candidate || !ice.candidate.candidate || !(ice.candidate.candidate.indexOf('typ relay')>-1))  return;
      promiseResolved = true;
      resolve(true);
    };
  });   
}
 $("#idForm").submit(function (e) {
$("#result").text("please wait.....")
	var url =  $("#url").val()
var username =  $("#username").val()
var password = $("#password").val()
  e.preventDefault();
alert("ok")
checkTURNServer({
    urls: url,
    username: username,
    credential: password
}).then(function(bool){
    if( bool){
$("#result").text("Turn server is active")}else{
$("#result").text("Turn server is Not active")
}}).catch(console.error.bind(console));

})

</script>
</body></html>
```
