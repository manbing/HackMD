# Linux Programming

## NL80211

``` C
int show_callback(struct nl_msg *msg, void *cb)
{
    struct nlattr *tb[NL80211_ATTR_MAX + 1];
    struct nlattr *vndr_tb[MTK_NL80211_VENDOR_ATTR_VENDOR_SHOW_ATTR_MAX + 1];
    struct genlmsghdr *gnlh = nlmsg_data(nlmsg_hdr(msg));
    char *show_str = NULL;
    int err = 0;

    err = nla_parse(tb, NL80211_ATTR_MAX, genlmsg_attrdata(gnlh, 0),
              genlmsg_attrlen(gnlh, 0), NULL);
    if (err < 0)
        return err;

    if (tb[NL80211_ATTR_VENDOR_DATA]) {
        err = nla_parse_nested(vndr_tb, MTK_NL80211_VENDOR_ATTR_VENDOR_SHOW_ATTR_MAX,
            tb[NL80211_ATTR_VENDOR_DATA], NULL);
        if (err < 0)
            return err;

        if (vndr_tb[MTK_NL80211_VENDOR_ATTR_VENDOR_SHOW_RSP_STR]) {
            show_str = nla_data(vndr_tb[MTK_NL80211_VENDOR_ATTR_VENDOR_SHOW_RSP_STR]);
            mac_table2 = (struct table *)show_str;
        }
    } else
        printf("no any show rsp string from driver\n");

    return 0;
}

static int get_ifindex(char *ifname)
{
    int fd = 0;
    struct ifreq ifr = {0};

    if ((fd = socket(AF_INET, SOCK_DGRAM, 0)) < 0) {
        perror("socket error");
        return 0;
    }

    snprintf(ifr.ifr_name, IFNAMSIZ, "%s", ifname);
    if (ioctl(fd, SIOCGIFINDEX, &ifr) < 0) {
        perror("SIOCGIFINDEX error");
        close(fd);
        return 0;
    }

    close(fd);
    return ifr.ifr_ifindex;
}


void nl80211_call(char *ifname, struct table *mac_table)
{
    struct unl unl = {0};
    struct nl_msg *msg = NULL;
    void *data;
    int ret = 0;

    if (unl_genl_init(&unl, "nl80211") < 0) {
        printf("Failed to connect to nl80211\n");
        return;
    }

    msg = unl_genl_msg(&unl, NL80211_CMD_VENDOR, false);
    nla_put_u32(msg, NL80211_ATTR_IFINDEX, get_ifindex(ifname));
    nla_put_u32(msg, NL80211_ATTR_VENDOR_ID, MTK_NL80211_VENDOR_ID);
    nla_put_u32(msg, NL80211_ATTR_VENDOR_SUBCMD, MTK_NL80211_VENDOR_SUBCMD_VENDOR_SHOW);

    data = nla_nest_start(msg, NL80211_ATTR_VENDOR_DATA);
    if (!data) {
        printf("nla_nest_start fail\n");
        return;
    }

    nla_put_string(msg, MTK_NL80211_VENDOR_ATTR_VENDOR_SHOW_CMD_STR, "stainfo=stdout");
    nla_nest_end(msg, data);

    mac_table2 = NULL;
    ret = unl_genl_request(&unl, msg, show_callback, NULL);
    if (ret) {
        printf("Command failed: %s (%d)\n", strerror(-ret), ret);
        goto out;
    }

    if (!mac_table2) {
        printf("unl_genl_request error\n");
        goto out;
    }

    memcpy(mac_table, mac_table2, sizeof(struct table));
out:
    unl_free(&unl);
    return;
}


```