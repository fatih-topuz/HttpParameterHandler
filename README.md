# HttpParameterHandler by Fatih Topuz


def urlparse(request):
    fields = request.split("\r\n")
    url=fields[0] #to get the method, http version and url parameters ie. GET /?paramter1=value1&parameter2=value2&parameter3=value3 HTTP/1.1
    urlcontent=url.split(" ")
    urlparams={}
    params={}
    urlparams["method"]=urlcontent[0]
    urlparams["httpversion"]=urlcontent[2]
    fields = fields[1:] #ignore the GET / HTTP/1.1

    for field in fields: #add other request parameters to urlparams
        if field=='':
            dummy=0
        else:
            key,value = field.split(': ')#split each line by http field name and value
            urlparams[key] = value

    urlcontent[1]=urlcontent[1].strip('/?') 
    urlcontent[1]=urlcontent[1].split("&")
    for parameter in urlcontent[1]: #after discarding parameter characters '/?' and "&", starting to form dict for url parameters
        if parameter=='':
            dummy=0
        else:
            key,value=parameter.split('=')
            params[key]=value
    return url,urlparams,params
    
request= 'GET /?parameter1=value1&parameter2=value2&parameter3=value3 HTTP/1.1\r\nHost: 127.0.0.1:8080\r\nConnection: keep-alive\r\nUpgrade-Insecure-Requests: 1\r\nUser-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 YaBrowser/18.10.1.872 Yowser/2.5 Safari/537.36\r\nAccept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8\r\nAccept-Encoding: gzip, deflate, br\r\nAccept-Language: tr,en;q=0.9,ca;q=0.8\r\n\r\n' #sample request from the browser

url,urlparams,params=urlparse(request) #params is for parameters in url, urlparams for http parameters

print (params['parameter1'])
print (params['parameter2'])
print (params['parameter3'])
print(params)


print (urlparams)
print (urlparams['method'])
print (urlparams['httpversion'])


