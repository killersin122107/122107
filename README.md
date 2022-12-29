# 122107
Project-assasin

# .github/vpn/client.ovpn
...
ca ca.crt
cert user.crt
key user.key
tls-auth tls.key 
auth-user-pass secret.txt
...
# .github/workflows/your-workflow

# .github/workflows/your-workflow.yml
...
jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - name: Install OpenVPN
        run: |
          sudo apt-get update
          sudo apt-get --assume-yes --no-install-recommends install openvpn          

      - name: Setup VPN config
        run: |
          echo "${{ secrets.CA_CRT }}" > ca.crt
          echo "${{ secrets.USER_CRT }}" > user.crt
          echo "${{ secrets.USER_KEY }}" > user.key
          echo "${{ secrets.SECRET_USERNAME_PASSWORD }}" > secret.txt
          echo "${{ secrets.TLS_KEY }}" > tls.key          

      - name: Connect VPN
        run: sudo openvpn --config ".github/vpn/config.ovpn" --log "vpn.log" --daemon

      - name: Wait for a VPN connection
        timeout-minutes: 1
        run: until ping -c1 your-server-address; do sleep 2; done
        # OR
        run: until dig @your-dns-resolver your-server-address A +time=1; do sleep 2; done

      - name: Deploy
        run: some-command
  
      - name: Kill VPN connection
        if: always()
        run: |
          sudo chmod 777 vpn.log
          sudo killall openvpn          

      - name: Upload VPN logs
        uses: actions/upload-artifact@v2
        if: always()
        with:
          name: VPN logs
          path: vpn.log
...




$ git clone https://github.com/killersin122107/122107.git
Username: killersin122107
Password: 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


$ git remote add origin https://github.com/killersin122107/122107.git
# Set a new remote

$ git remote -v
# Verify new remote
> origin  https://github.com/killersin122107/122107.git (fetch)
> origin  https://github.com/killersin122107/122107.git (push)

$ git remote add origin git@github.com:killersin122107/122107.git
# Set a new remote

$ git remote -v
# Verify new remote
> origin  git@github.com:killersin122107/122107.git (fetch)
> origin  git@github.com:killersin122107/122107.git (push)
