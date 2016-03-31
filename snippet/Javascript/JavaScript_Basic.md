### 第一章
完整的 JavaScript 实现应该由下列三个不同调那个的部分组成
+ 核心ECMAScript ,提供访问和操作网页内容的方法和接口.
+ 文档对象模型 DOM ,提供与浏览器交互的方法和接口.
+ 浏览器对象模型 BOM ,提供与浏览器交互的方法和接口.



PATH=${PATH}:/usr/local/bin
IFS=$'\n'

function generateIcon () {
  BASE_IMAGE_NAME=$1

  TARGET_PATH="${BUILT_PRODUCTS_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/${BASE_IMAGE_NAME}"
  BASE_IMAGE_PATH=$(find /Users/Ezrohir/Desktop -name ${BASE_IMAGE_NAME})
  cp -f ${BASE_IMAGE_PATH} ${TARGET_PATH}
}

generateIcon "AppIcon60x60@2x.png"
generateIcon "AppIcon60x60@3x.png"
