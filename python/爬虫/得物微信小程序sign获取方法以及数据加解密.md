# 1.获取sign

```javascript
e.default = function() {
                var t = arguments.length > 0 && void 0 !== arguments[0] ? arguments[0] : {}
                  , e = ""
                  , n = function(t) {
                    if (null === t)
                        return "";
                    if (t instanceof Array) {
                        var e = "";
                        return t.forEach(function(t) {
                            e.length > 0 && (e += ","),
                            e += JSON.stringify(t);
                        }),
                        e;
                    }
                    return t instanceof Object ? JSON.stringify(t) : t.toString();
                };
                return e = Object.keys(t).sort().reduce(function(e, r) {
                    return void 0 === t[r] ? e : "".concat(e).concat(r).concat(n(t[r]));
                }, ""),
                e += "19bc545a393a25177083d4a748807cc0",//isAggr1limit20page0showHot1sortMode1sortType0titleTS270
                Object(r.md5)(e);
            }
// 搜索商品的请求参数：
isAggr: 1
limit: 20
page: 0
showHot: 1
sortMode: 1
sortType: 0
title: "TS270"
//
```



# 2. 请求参数加密

0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z,a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z