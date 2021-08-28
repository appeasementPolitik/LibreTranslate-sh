# LibreTranslate-sh
Unix bindings for LibreTranslate

# Examples
```
./libretranslate translate en es "Hello World"
{"translatedText":"Hola Mundo"}
```

Detect language:
```
./libretranslate detect "Hello World this is in English"
[{"confidence":96.0,"language":"en"}]
```

Get available languages:
```
./libretranslate languages
[{"code":"en","name":"English"},{"code":"ar","name":"Arabic"},{"code":"zh","name":"Chinese"},{"code":"fr","name":"French"},{"code":"de","name":"German"},{"code":"hi","name":"Hindi"},{"code":"id","name":"Indonesian"},{"code":"ga","name":"Irish"},{"code":"it","name":"Italian"},{"code":"ja","name":"Japanese"},{"code":"ko","name":"Korean"},{"code":"pl","name":"Polish"},{"code":"pt","name":"Portuguese"},{"code":"ru","name":"Russian"},{"code":"es","name":"Spanish"},{"code":"tr","name":"Turkish"},{"code":"vi","name":"Vietnamese"}]
```

Set URL:
```
export LIBRETRANSLATE_URL="https://translate.argosopentech.com/"

```

Set API Key:
```
export LIBRETRANSLATE_API_KEY="<my-api-key>"

```

Format with jq:
```
./libretranslate translate en es "Hello World" | jq '.'
{
  "translatedText": "Hola Mundo"
}

```

Parse with jq:
```
./libretranslate translate en es "Hello World" | jq '.translatedText'
"Hola Mundo"

```

# Dependencies
[cURL](https://curl.se/)
```
sudo apt install curl
```

[jq](https://stedolan.github.io/jq/) (optional)
```
sudo apt install jq
```

# Source
```
if [ $LIBRETRANSLATE_URL ]
then
    url=$LIBRETRANSLATE_URL
else
    url="https://translate.argosopentech.com/"
fi

if [ $LIBRETRANSLATE_API_KEY ]
then
    api_key_flag="-F \"api_key=${$LIBRETRANSLATE_API_KEY}\""
else
    api_key_flag=""
fi

curl_args="--silent --show-error "

fun=$1
if [ $fun == "translate" ]
then
    source=$2
    target=$3
    q=$4
    curl -X POST \
            $curl_args \
            -F "q=$q" \
            -F "source=$source" \
            -F "target=$target" \
	    $api_key_flag \
            "${url}translate"
elif [ $fun == "detect" ]
then
    q=$2
    curl -X POST \
            $curl_args \
            -F "q=$q" \
	    $api_key_flag \
            "${url}detect"
elif [ $fun == "languages" ]
then
    curl -X POST \
            $curl_args \
            "${url}languages"
else
    echo "Invalid function"
fi

```