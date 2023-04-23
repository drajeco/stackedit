
Reference: [Github](https://github.com/benweet/stackedit/issues/1755)

For the time being, I developed a workaround. I know there already is  [#1724](https://github.com/benweet/stackedit/pull/1724)  that fixes the issue, but I wanted to keep using  [StackEdit](https://github.com/benweet/stackedit/issues/stackedit.io)  whilst the request is not yet merged.

Once you are on the screen asking you to "Grant access to your private repositories," open the developer console and paste the following lines.

window.XMLHttpRequest =  class MyXMLHttpRequest extends window.XMLHttpRequest {
  open(...args){
    if(args[1].startsWith("https://api.github.com/user?access_token=")) {
      // apply fix as described by github
      // https://developer.github.com/changes/2020-02-10-deprecating-auth-through-query-param/#changes-to-make
  
      const segments = args[1].split("?");
      args[1] = segments[0]; // remove query params from url
      const token = segments[1].split("=")[1]; // save the token
      
      const ret = super.open(...args);
      
      this.setRequestHeader("Authorization", `token ${token}`); // set required header
      
      return ret;
    }
    else {
      return super.open(...args);
    }
  }
}
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NTU0OTI1ODNdfQ==
-->