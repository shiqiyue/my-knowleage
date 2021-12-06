
## shopify页面嵌入脚本

```
用途，在用户的shopify页面嵌入对应的脚本；主要用于不修改用户的主题情况下，修改用户的页面(比如产品详情页面，增加按钮或者增加输入框)

shopify admin接口：https://shopify.dev/api/admin-rest/2021-10/resources/scripttag#[put]/admin/api/2021-10/script_tags/{script_tag_id}.json

shopify请求脚本时候，会带上shop参数(当前店铺的域名:wwy-test3.myshopify.com)
```
### 脚本示例
```js
window.SPOD = window.SPOD || {};
window.SPOD.CustomizeProduct = (function () {

    var URL = "https://wwy-test3.myshopify.com/pages/customizer";
    var CUSTOMIZABLE_TAG = "customizable";
    var SPOD_TAG = "SPOD";
    var BUTTON_ID = "spod-customizable-button";
    var BUTTON_TEXT = "Customize";

    var CustomizeProduct = {};

    CustomizeProduct.init = function () {
        var productHandle = CustomizeProduct.ProductUtil.findHandle();
        if (!productHandle) {
            warn("Could not find handle");
            return;
        }

        CustomizeProduct.ProductUtil.getProduct(productHandle, function (err, product) {
            if (err || !product) {
                warn("Error while getting product");
                return;
            }

            var customizableButton = CustomizeProduct.ButtonUtil.getButton();
            var hasCustomizableTag = CustomizeProduct.ProductUtil.hasRequiredTags(product);
            if (!hasCustomizableTag) {
                info("product has no customizable tag");
                customizableButton && customizableButton.remove();
                return;
            }

            if (!customizableButton) {
                customizableButton = CustomizeProduct.ButtonUtil.applyButton();
            }

            if (!customizableButton) {
                warn("Error while applying button");
                return;
            }

            customizableButton.setAttribute("visible", "");
            CustomizeProduct.ButtonUtil.applyEventListener(customizableButton, product);
        })
    };

    CustomizeProduct.ProductUtil = {
        findHandle: function () {
            var nodeList = document.querySelectorAll("link[type='application/json+oembed']");
            var linkElements = Array.prototype.slice.call(nodeList);

            var srcAttributes = linkElements.map(function (link) {
                return link.getAttribute("src");
            });

            srcAttributes = srcAttributes.filter(function (attribute) {
                return !!attribute;
            });

            var links = srcAttributes.concat([location.pathname]);

            for (var i = 0; i < links.length; i++) {
                var match = CustomizeProduct.Regex.productHandleRegex.exec(links[i]);
                if (match) {
                    return match[1];
                }
            }
        },
        getProduct: function (handle, callback) {
            var jsonProduct;
            if (window.json_product) {
                jsonProduct = window.json_product;
                if (jsonProduct && jsonProduct.tags && jsonProduct.variants) {
                    return callback && callback(null, jsonProduct);
                }
            }

            var script = document.getElementById("ProductJson-product-template");
            if (script) {
                try {
                    jsonProduct = JSON.parse(script.innerText);
                    if (jsonProduct && jsonProduct.tags && jsonProduct.variants) {
                        return callback && callback(null, jsonProduct);
                    }
                } catch (e) {
                    console.log(e);
                }
            }

            fetch("/products/" + handle + ".json")
                .then(function (response) {
                    return response.json();
                })
                .then(function (value) {
                    var product = value.product;
                    if (product && product.tags && product.variants) {
                        callback && callback(null, product);
                    } else {
                        callback && callback(true);
                    }
                }, callback)
        },
        hasRequiredTags: function (product) {
            if (!product || !product.tags) {
                return null;
            }

            var tags = Array.isArray(product.tags) ? product.tags : product.tags.split(",");
            var requiredTags = tags.filter(function (tag) {
                var currentTag = tag.trim().toLowerCase();
                return currentTag === CUSTOMIZABLE_TAG.toLowerCase() || currentTag === SPOD_TAG.toLowerCase();
            });

            return requiredTags.length >= 2;
        }
    };

    CustomizeProduct.ButtonUtil = {
        getButton: function () {
            return document.querySelector("#" + BUTTON_ID);
        },
        getAddToCarTButton: function () {
            return document.querySelector("form[action='/cart/add'] [type='submit'], form[action^='/cart/add'] [type='submit']");
        },
        applyButton: function () {
            var addToCartButton = CustomizeProduct.ButtonUtil.getAddToCarTButton();
            if (!addToCartButton) {
                warn("Could not find cart button");
                return null;
            }

            var cartButtonNode = addToCartButton.cloneNode(false);
            cartButtonNode.innerText = BUTTON_TEXT;
            if (cartButtonNode.tagName.toLowerCase() === "input") {
                cartButtonNode.setAttribute("value", BUTTON_TEXT);
            }
            cartButtonNode.setAttribute("id", BUTTON_ID);
            cartButtonNode.setAttribute("name", BUTTON_ID);
            cartButtonNode.setAttribute("type", "button");
            cartButtonNode.removeAttribute("data-action");
            cartButtonNode.removeAttribute("data-add-to-cart");
            cartButtonNode.style.marginTop = "10px";
            addToCartButton.parentNode.appendChild(cartButtonNode);

            return CustomizeProduct.ButtonUtil.getButton();
        },
        applyEventListener: function (button, product) {
            button && button.addEventListener("click", function (event) {
                event && event.preventDefault();

                var query = window.location && window.location.search,
                    variantRegex = query.match(CustomizeProduct.Regex.variantRegex);

                var found = query.match(variantRegex);
                var variant;
                if (found && !!found.length) {
                    var variantId = found[0];
                    variant = product.variants.find(function (variant) {
                        return variant.id === variantId;
                    });
                } else {
                    variant = product.variants[0];
                }

                window.location.href = URL + (URL.indexOf("?") === -1 ? "?" : "&") + "sku=" + variant.sku;
            })
        }
    };

    CustomizeProduct.Regex = {
        productHandleRegex: /products\/([^/]+)(?:$|\.)/,
        variantRegex: /variant=([0-9]+)/
    };

    function warn(msg) {
        try {
            var debugMode = window.location.href.match(/([?&])debug/);
            debugMode && console.warn("SPOD: " + msg);
        } catch (e) {
        }
    }

    function info(msg) {
        try {
            var debugMode = window.location.href.match(/([?&])debug/);
            debugMode && console.info("SPOD: " + msg);
        } catch (e) {
        }
    }

    CustomizeProduct.init();

    return CustomizeProduct;
})();
```
