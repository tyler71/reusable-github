# Setup Bitwarden Cli


## Usage

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| bw-clientid | String | yes | BitWarden Client ID - [Docs](https://bitwarden.com/help/personal-api-key/#get-your-personal-api-key) |
| bw-clientsecret | String | yes | BitWarden Client Secret - [Docs](https://bitwarden.com/help/personal-api-key/#get-your-personal-api-key) |
| bw-password | String | yes | Master Password |
| bw-host | String | no  | Self Hosted URL |

### Session key
To use items in the vault, a [session key](https://bitwarden.com/help/cli/#using-a-session-key) must available to the client `bw`  
To access this key, use the `session-key` output variable.  
For example, with a step id of `bitwarden`:
```yaml
${{ steps.bitwarden.outputs.session-key }}
```


## Example

```yaml
- name: Setup BitWarden
  id: bitwarden
  uses: tyler71/setup-bitwarden@master
  with:
    bw-clientid: ${{ secrets.BW_CLIENTID }}
    bw-clientsecret: ${{ secrets.BW_CLIENTSECRET }}
    bw-password: ${{ secrets.BW_PASSWORD }}
    bw-host: ${{ vars.BW_HOST }}
- name: List bitwarden items
  env:
    BW_SESSION: ${{ steps.bitwarden.outputs.session-key }}
  run: |
    bw list items | jq .

- name: Get bitwarden item
  run: |
    bw get item d47f8f16-c235-4cda-ad55-440dec080526 --session ${{ steps.bitwarden.outputs.session-key }} | jq -r .login.username
    
```
