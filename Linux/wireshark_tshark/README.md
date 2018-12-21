# Wireshark / tshark Notes

#### Quick reference

In wireshak we can do firstler as we do in wiresak by using the `-f`
`tshark -f "ip src 192.168.1.2 && tcp"`
when using multiple filter we can specify it by using an `&&` or `and`

If we need to filter out something, like all request on port 443 we do:
`tshark -f "not port 443"`
The `not` in front of any filter will allow us to do the negative of any filter.
