name: Generate bestcf
on:
  workflow_dispatch:
  schedule:
    - cron: "0 */12 * * *"
  push:
    branches:
      - main
    paths-ignore:
      - "**/README.md"
      - "heartbeat.txt"  # 忽略心跳文件，防止 push 触发死循环

jobs:
  build:
    runs-on: self-hosted # 强制在你的本地 Windows 运行器上执行
    permissions:
      contents: write  # 用于推送分支和创建 Release
      actions: write   # 关键：用于删除旧的工作流运行记录
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0 # 获取完整历史以确保 push 正常

      - name: Install dependencies
        run: pip install requests

      - name: Generate bestcf `Domain`
        shell: bash
        run: |
          # 设置版本号 (UTC+8 偏移，如果是 Windows 本地环境通常直接 date 即可)
          echo "update_version=$(date +%Y-%m-%d)" >> ${GITHUB_ENV}
          mkdir -p ./tmp/ ./bestcf/
          
          curl -sSL --ssl-no-revoke https://cf.090227.xyz | grep 'copyDomain' | awk -F "'" '{print $2}' > ./tmp/temp-bestcf-domain.txt
          curl -sSL --ssl-no-revoke https://cf.090227.xyz | sed -n '/<pre>/,/<\/pre>/p' | sed 's/<pre>//; s/<\/pre>//; s/^[ \t]*//; s/#.*//' >> ./tmp/temp-bestcf-domain.txt
          curl -sSL --ssl-no-revoke https://vps789.com/openApi/cfIpTop20 | jq -r '.data.good[].ip' | grep -Ev '^[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+$' >> ./tmp/temp-bestcf-domain.txt
          curl -sSL --ssl-no-revoke https://www.wetest.vip/page/cloudflare/cname.html | awk '/4&amp;6/{print prev} {prev=$0}' | awk -F '[<>]' '{print $3}' | sed 's/\*/bestcf/' >> ./tmp/temp-bestcf-domain.txt
          
          cat ./tmp/temp-bestcf-domain.txt | grep -E 'cloudflare|cf' | /usr/bin/sort -f -u > ./bestcf/bestcf-domain.txt
          rm -rf ./tmp*

      - name: Generate bestcf `IP`
        shell: bash
        run: |
          # CMCC-IP
          curl -sSL --ssl-no-revoke "https://cf.090227.xyz/cmcc?ips=50" | sed 's/#.*//' | awk '{print $0 "#CMCC-IPv4_CMLiu_" NR}' > ./bestcf/cmcc-ip.txt
          curl -sSL --ssl-no-revoke "https://cf.090227.xyz/cmcc-ipv6?ips=50" | sed 's/#.*//' | awk '{print $0 "#CMCC-IPv6_CMLiu_" NR}' >> ./bestcf/cmcc-ip.txt
          curl -sSL --ssl-no-revoke https://vps789.com/openApi/cfIpApi | jq -r '.data.CM[].ip' | awk '{print $0 "#CMCC-IPv4_VPS789_" NR}' >> ./bestcf/cmcc-ip.txt
          curl -sSL --ssl-no-revoke https://api.hostmonit.com/get_optimization_ip --data-raw '{"key":"iDetkOys"}' | jq -r '.info[] | select(.line == "CM") | .ip' | awk '{print $0 "#CMCC-IPv4_CFYes_" NR}' >> ./bestcf/cmcc-ip.txt
          curl -sSL --ssl-no-revoke https://api.hostmonit.com/get_optimization_ip --data-raw '{"key":"iDetkOys","type":"v6"}' | jq -r '.info[] | select(.line == "CM") | .ip' | awk '{print "["$0"]#CMCC-IPv6_CFYes_" NR}' >> ./bestcf/cmcc-ip.txt
          curl -sSL --ssl-no-revoke https://www.wetest.vip/page/cloudflare/address_v4.html | awk '/移动/{getline; print}' | awk -F '[<>]' '{print $3}' | awk '{print $0 "#CMCC-IPv4_WeTest_" NR}' >> ./bestcf/cmcc-ip.txt
          curl -sSL --ssl-no-revoke https://www.wetest.vip/page/cloudflare/address_v6.html | awk '/移动/{getline; print}' | awk -F '[<>]' '{print $3}' | awk '{print "["$0"]#CMCC-IPv6_WeTest_" NR}' >> ./bestcf/cmcc-ip.txt

          # CUCC-IP
          curl -sSL --ssl-no-revoke "https://cf.090227.xyz/cu?ips=50" | sed 's/#.*//' | awk '{print $0 "#CUCC-IPv4_CMLiu_" NR}' > ./bestcf/cucc-ip.txt
          curl -sSL --ssl-no-revoke https://vps789.com/openApi/cfIpApi | jq -r '.data.CU[].ip' | awk '{print $0 "#CUCC-IPv4_VPS789_" NR}' >> ./bestcf/cucc-ip.txt
          curl -sSL --ssl-no-revoke https://api.hostmonit.com/get_optimization_ip --data-raw '{"key":"iDetkOys"}' | jq -r '.info[] | select(.line == "CU") | .ip' | awk '{print $0 "#CUCC-IPv4_CFYes_" NR}' >> ./bestcf/cucc-ip.txt
          curl -sSL --ssl-no-revoke https://api.hostmonit.com/get_optimization_ip --data-raw '{"key":"iDetkOys","type":"v6"}' | jq -r '.info[] | select(.line == "CU") | .ip' | awk '{print "["$0"]#CUCC-IPv6_CFYes_" NR}' >> ./bestcf/cucc-ip.txt
          curl -sSL --ssl-no-revoke https://www.wetest.vip/page/cloudflare/address_v4.html | awk '/联通/{getline; print}' | awk -F '[<>]' '{print $3}' | awk '{print $0 "#CUCC-IPv4_WeTest_" NR}' >> ./bestcf/cucc-ip.txt
          curl -sSL --ssl-no-revoke https://www.wetest.vip/page/cloudflare/address_v6.html | awk '/联通/{getline; print}' | awk -F '[<>]' '{print $3}' | awk '{print "["$0"]#CUCC-IPv6_WeTest_" NR}' >> ./bestcf/cucc-ip.txt

          # CTCC-IP
          curl -sSL --ssl-no-revoke "https://cf.090227.xyz/ct?ips=50" | sed 's/#.*//' | awk '{print $0 "#CTCC-IPv4_CMLiu_" NR}' > ./bestcf/ctcc-ip.txt
          curl -sSL --ssl-no-revoke https://vps789.com/openApi/cfIpApi | jq -r '.data.CT[].ip' | awk '{print $0 "#CTCC-IPv4_VPS789_" NR}' >> ./bestcf/ctcc-ip.txt
          curl -sSL --ssl-no-revoke https://api.hostmonit.com/get_optimization_ip --data-raw '{"key":"iDetkOys"}' | jq -r '.info[] | select(.line == "CT") | .ip' | awk '{print $0 "#CTCC-IPv4_CFYes_" NR}' >> ./bestcf/ctcc-ip.txt
          curl -sSL --ssl-no-revoke https://api.hostmonit.com/get_optimization_ip --data-raw '{"key":"iDetkOys","type":"v6"}' | jq -r '.info[] | select(.line == "CT") | .ip' | awk '{print "["$0"]#CTCC-IPv6_CFYes_" NR}' >> ./bestcf/ctcc-ip.txt
          curl -sSL --ssl-no-revoke https://www.wetest.vip/page/cloudflare/address_v4.html | awk '/电信/{getline; print}' | awk -F '[<>]' '{print $3}' | awk '{print $0 "#CTCC-IPv4_WeTest_" NR}' >> ./bestcf/ctcc-ip.txt
          curl -sSL --ssl-no-revoke https://www.wetest.vip/page/cloudflare/address_v6.html | awk '/电信/{getline; print}' | awk -F '[<>]' '{print $3}' | awk '{print "["$0"]#CTCC-IPv6_WeTest_" NR}' >> ./bestcf/ctcc-ip.txt

          # CF-IP & Proxy-IP
          curl -sSL --ssl-no-revoke https://vps789.com/openApi/cfIpApi | jq -r '.data.AllAvg[].ip' | awk '{print $0 "#CF-IPv4_VPS789_" NR}' > ./bestcf/bestcf-ip.txt
          curl -sSL --ssl-no-revoke https://ip.164746.xyz/ipTop10.html | tr ',' '\n' | head -n 10 | awk '{print $0 "#CF-IPv4_CFSpeedTest_" NR}' >> ./bestcf/bestcf-ip.txt
          curl -sSL --ssl-no-revoke "https://ipdb.api.030101.xyz/?type=bestcfv4" | head -n 10 | awk '{print $0 "#CF-IPv4_IPDB_" NR}' >> ./bestcf/bestcf-ip.txt
          curl -sSL --ssl-no-revoke "https://ipdb.api.030101.xyz/?type=bestcfv6" | head -n 10 | awk '{print "["$0"]#CF-IPv6_IPDB_" NR}' >> ./bestcf/bestcf-ip.txt
          curl -sSL --ssl-no-revoke "https://ipdb.api.030101.xyz/?type=bestproxy" | head -n 10 | awk '{print $0 "#Proxy-IPv4_IPDB_" NR}' > ./bestcf/proxy-ip.txt

      - name: Auto-Country Classification
        env:
          PYTHONIOENCODING: utf-8
        run: python filter.py

      - name: Git push to bestcf branch
        shell: bash
        run: |
          cd ./bestcf/ || exit 1
          git init
          git config --local user.name "github-actions[bot]"
          git config --local user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git checkout -b bestcf
          git add .
          git commit -m "优选更新于 ${{ env.update_version }}"
          git remote add origin "https://${{ github.actor }}:${{ secrets.GITHUB_TOKEN }}@github.com/${{ github.repository }}"
          git push -f origin bestcf

      - name: Create & Update GitHub Release
        uses: softprops/action-gh-release@v1
        with:
          tag_name: update-${{ env.update_version }}
          name: 优选更新 - ${{ env.update_version }}
          files: |
            ./bestcf/*.txt
          body: |
            Cloudflare 优选 IP 自动更新。
            更新时间：${{ env.update_version }} (UTC)
          draft: false
          prerelease: false
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - name: Purge jsDelivr CDN
        shell: bash
        run: |
          cd ./bestcf/ || exit 1
          /usr/bin/find . -maxdepth 5 -name "*.txt" | sed 's|^\./||' | while read file; do
            curl -sSL --ssl-no-revoke "https://purge.jsdelivr.net/gh/${{ github.repository }}@bestcf/${file}"
          done

      - name: Keepalive (Commit to Main)
        shell: bash
        run: |
          # 这一步是为了让 main 分支产生提交，防止 Actions 被禁用
          git config --local user.name "github-actions[bot]"
          git config --local user.email "41898282+github-actions[bot]@users.noreply.github.com"
          echo "Last active: $(date)" > heartbeat.txt
          git add heartbeat.txt
          git commit -m "Chore: Keepalive heartbeat $(date)" || exit 0
          git push origin main

      - name: Delete old workflow runs
        uses: Mattraks/delete-workflow-runs@v2
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          repository: ${{ github.repository }}
          retain_days: 1 # Windows 本地运行器建议缩短保留时间，节省资源
          keep_minimum_runs: 1
