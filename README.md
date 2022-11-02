### API Optimization

RPC

gRPC

REST 

SOAP

GRAPHQL

EVENT DRIVE DATA ARCHITECTURE

### PHP MULTICURL CALL
STATIC
```

        $url1 = "https://portfolio.iwebitechnology.xyz/wp-json/wp/v2/media";
        $url2 = "https://portfolio.iwebitechnology.xyz/wp-json/wp/v2/posts";

        $multiCurl = curl_multi_init();

                $options = array(
                        CURLOPT_RETURNTRANSFER => true, // return web page
                        CURLOPT_HEADER => false, // don't return headers
                        CURLOPT_FOLLOWLOCATION => true, // follow redirects
                        CURLOPT_MAXREDIRS => 10, // stop after 10 redirects
                        CURLOPT_ENCODING => "utf8", // handle compressed
                        CURLOPT_USERAGENT => isset($_SERVER['HTTP_USER_AGENT']) ? $_SERVER['HTTP_USER_AGENT'] : "portfolio.iwebitechnology.xyz", // name of client
                        CURLOPT_AUTOREFERER => true, // set referrer on redirect
                        CURLOPT_CONNECTTIMEOUT => 30, // time-out on connect
                        CURLOPT_TIMEOUT => 30, // time-out on response
                );
        $curl1 = curl_init();
        $curl2 = curl_init();

        curl_setopt_array($curl1, $options); //SET CURLOPT FROM ARRAY
        curl_setopt_array($curl2, $options);

        curl_setopt($curl1, CURLOPT_URL, $url1); //Add URL to CURLOPT_URL
        curl_setopt($curl2, CURLOPT_URL, $url2);

        $multiCurl = curl_multi_init();
        curl_multi_add_handle($multiCurl, $curl1); //ADD CURL TO MULTI CURL
        curl_multi_add_handle($multiCurl, $curl2);

        $running = null;
        do {
                $status = curl_multi_exec($multiCurl, $running);
                if($status){
                        curl_multi_select($multiCurl);
                }

        } while( $running > 0 );

        $response1 = curl_multi_getcontent($curl1);
        $response2 = curl_multi_getcontent($curl2);

        curl_multi_remove_handle($multiCurl, $curl1);
        curl_multi_remove_handle($multiCurl, $curl2);

        curl_multi_close($multiCurl);

        $data1 = json_decode($response1, true);
        $data2 = json_decode($response2, true);

```
DYNAMIC
```
        $urls = array("https://ilc.upd.edu.ph/wp-json/wp/v2/media", "https://ilc.upd.edu.ph/wp-json/wp/v2/posts");
        $options = array(
                        CURLOPT_RETURNTRANSFER => true, // return web page
                        CURLOPT_HEADER => false, // don't return headers
                        CURLOPT_FOLLOWLOCATION => true, // follow redirects
                        CURLOPT_MAXREDIRS => 10, // stop after 10 redirects
                        CURLOPT_ENCODING => "utf8", // handle compressed
                        CURLOPT_USERAGENT => isset($_SERVER['HTTP_USER_AGENT']) ? $_SERVER['HTTP_USER_AGENT'] : "portfolio.iwebitechnology.xyz", // name of client
                        CURLOPT_AUTOREFERER => true, // set referrer on redirect
                        CURLOPT_CONNECTTIMEOUT => 30, // time-out on connect
                        CURLOPT_TIMEOUT => 30, // time-out on response
                );
        $curls = array();

        $multiCurl = curl_multi_init();
        foreach($urls as $key => $url){
                $curls[$key] = curl_init(); //ASSIGNING curl_init TO ARRAY $CURL[0] = curl_init();
                curl_setopt_array($curls[$key], $options);
                curl_setopt($curls[$key], CURLOPT_URL, $url);
                curl_multi_add_handle($multiCurl, $curls[$key]);
        }

        $running = null;
        do {
                $status = curl_multi_exec($multiCurl, $running);
                if($status){
                        curl_multi_select($multiCurl);
                }

        } while( $running > 0 );

        $responses = array();
        foreach($curls as $key => $curl) //$curls now have [0]=curl_init and [1]=curl_init
        {
                $responses[$key] = curl_multi_getcontent($curl);
                curl_multi_remove_handle($multiCurl, $curl);
        }

        curl_multi_close($multiCurl);

        $datas = array();
        foreach($responses as $key => $response){
                $datas[$key] = json_decode($response, true);
        }

//Media
        var_dump($datas[0]);
//Articles
        var_dump($datas[1]);
```
### PHP SINGLE CURL
```
$url = "https://portfolio.iwebitechnology.xyz/wp-json/wp/v2/posts/?_embed&per_page=3";

$options = array(
         		CURLOPT_RETURNTRANSFER => true, // return web page
         		CURLOPT_HEADER => false, // don't return headers
         		CURLOPT_FOLLOWLOCATION => true, // follow redirects
         		CURLOPT_MAXREDIRS => 10, // stop after 10 redirects
         		CURLOPT_ENCODING => "utf8", // handle compressed
         		CURLOPT_USERAGENT => isset($_SERVER['HTTP_USER_AGENT']) ? $_SERVER['HTTP_USER_AGENT'] : "portfolio.iwebitechnology.xyz", // name of client
         		CURLOPT_AUTOREFERER => true, // set referrer on redirect
    			CURLOPT_CONNECTTIMEOUT => 30, // time-out on connect
    			CURLOPT_TIMEOUT => 30, // time-out on response
    		);
    		$curl = curl_init();
    		curl_setopt($curl, CURLOPT_URL, $url);
    //curl_setopt($curl, CURLOPT_POST, 1);
    		curl_setopt_array($curl, $options);
    //curl_setopt($curl, CURLOPT_POSTFIELDS, json_encode($credentials));
    //$errmsg = curl_error($curl);
    // $cinfo = curl_getinfo($curl);
    		$response = curl_exec($curl);
    		if(curl_errno($curl)){
        		throw new Exception(curl_error($curl));
    		}
    		curl_close($curl);
    		$data = json_decode($response, true);

```
### Tutorial

[Comparing web API](https://youtu.be/NFw0HznpLlM)

### References

[Best Practices Creating API](https://stackoverflow.blog/2021/10/06/best-practices-for-authentication-and-authorization-for-rest-apis/)
