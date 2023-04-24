## Part 1

Code for `StringServer`:
```
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    String str = "";

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return str;
        } 
        else {
            System.out.println("Path: " + url.getPath());
            if (url.getPath().contains("/add-message")){
                String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("s")) {
                    str += "\n" + parameters[1];
                    return str;
                }
            }
            return "404 Not Found";
        }
    }
}

class SearchEngine {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

Examples of using `/add-message`:  
![Image](Add_Apple.png)  
* The one method called was `handleRequest` which determines the course of action depending on the URI.
* The relevant argument of `url` of method `handleRequest` is what is typed in the search bar, determining what is displayed on the page. For this example `url` is set to `http://localhost:4011/add-message?s=apple`. The field `str` in class `Handler` is what is displayed on the page and is first set to an empty string `""`.
* From this specific request the `str` field changes to a string `"\napple"` due to the `handleRequest` method adding a `"\n"` and the string found after the `"="` in `url` which in this case is `"apple"`.  
![Image](Add_Banana.png)  
* The method called in the screenshot was `handleRequest` which determines the course of action depending on the URI.
* The relevant argument `url` from method `handleRequest` is what is typed in the search bar, determining what is deplayed on the page. For this example `url` is set to `http://localhost:4011/add-message?s=banana`. The field `str` in class `Handler` is what is displayed on the page and is set as `"\n apple"` before the method call.
* From this specific request, the `str` field changes to a string `"\napple\nbanana"` due to the `handleRequest` method adding a `"\n"` and the string found after the `"+"` in `url` which in this case is `"banana"`.
