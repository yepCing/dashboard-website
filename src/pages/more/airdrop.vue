<template>
  <div style="background: #000">
    <div class="airdrop" @scroll="onScroll">
      <img
        class="airdrop-bg"
        width="100%"
        src="/img/airDrop/bnb-airdrop-bg.png"
        alt=""
      />
      <nav-header :scrollTop="scrollTop"></nav-header>
      <div class="airdrop-body">
        <div class="airdrop-intro al-c space-btw">
          <div class="airdrop-desc">
            <div class="mb-2 fw-b text-white" style="font-size: 24px">
              Elite Quest
            </div>
            <div class="fz-14">
              Welcome to the 4EVERLAND Elite Quest, your go-to destination for
              collaborative activities that reward you with $4EVER Points! By
              working together with our esteemed alliance partners, we are
              excited to offer a series of enriching experiences designed to
              engage and reward our community members.
            </div>
          </div>

          <div class="airdrop-card-container">
            <div class="airdrop-card pa-6">
              <div class="card-title fw-b">MEMBERS PROFILE</div>
              <div class="al-c space-btw">
                <div class="flex-1">
                  <div class="fw-b text-white" style="font-size: 24px">
                    {{ addr }}
                  </div>
                  <div class="bind-type mt-2">
                    <img
                      :src="
                        x ? '/img/airDrop/x-active.svg' : '/img/airDrop/x.svg'
                      "
                      width="24"
                      alt=""
                    />
                    <img
                      :src="
                        dc
                          ? '/img/airDrop/dc-active.svg'
                          : '/img/airDrop/dc.svg'
                      "
                      class="mx-2"
                      width="24"
                      alt=""
                    />
                    <img
                      :src="
                        onChain
                          ? '/img/airDrop/auth-active.svg'
                          : '/img/airDrop/auth.svg'
                      "
                      width="24"
                      alt=""
                    />
                  </div>
                  <div class="d-flex al-end space-btw">
                    <div>
                      <div class="fz-12">Points</div>
                      <div class="fw-b fz-20 text-white">{{ points }}</div>
                    </div>
                    <div>
                      <div class="fz-12">Tasks</div>
                      <div class="fw-b fz-20 text-white">{{ tasks }}</div>
                    </div>
                    <div>
                      <div class="fz-12">Ranking</div>
                      <div class="fw-b fz-20 text-white">{{ rank }}</div>
                    </div>
                  </div>
                </div>
                <img
                  class="card-avatar ml-8"
                  v-if="info.avatar"
                  :src="info.avatar"
                  alt=""
                />
                <div class="card-avatar ml-8 al-c flex-center" v-else>
                  <img src="/img/airDrop/unknown.svg" width="32" alt="" />
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="airdrop-task mt-10">
          <div class="fw-b text-white" style="font-size: 24px">Campaigns</div>
          <div class="mt-2 fz-14">
            Check the campaign details, complete the corresponding tasks, and
            earn points!
          </div>
          <div class="task-list mt-6">
            <div class="task-item d-flex mb-4">
              <div class="pa-6 task-img-wrap">
                <img
                  src="/img/airDrop/bnb-airdrop-activity.png"
                  width="100%"
                  alt=""
                />
              </div>
              <div class="pa-6 task-desc">
                <div class="fz-20 fw-b cursor-p text-white" @click="handleBnB">
                  BNB CHAIN AIRDROP ALLIANCE PROGRAM
                </div>
                <div class="fz-14 mt-4">
                  The BNB Chain Airdrop Alliance Program serves as a gesture of
                  gratitude towards the supportive BNB Chain community. Through
                  this program, 4EVERLAND is partnering with BNB Chain to
                  introduce exclusive rewards for retroactive users on BNB Smart
                  Chain (BSC) and opBNB.
                </div>
                <div class="mt-8 al-c space-btw">
                  <div class="reward fz-20 fw-b">
                    Shared Reward of 15 Million Points
                  </div>
                  <v-btn color="#039CFF" @click="handleBnB">
                    <span class="fw-b" style="color: #fff">Let's Go</span>
                  </v-btn>
                </div>
              </div>
            </div>
            <div class="task-item d-flex mb-4">
              <div class="pa-6 task-img-wrap">
                <img src="/img/airDrop/commingsoon.png" width="100%" alt="" />
              </div>
              <div class="pa-6 task-desc">
                <div class="fz-20 fw-b cursor-p text-white">TO BE REVEALED</div>
                <div class="fz-14 mt-4">
                  More thrilling activities coming your way soon. Stay tuned for
                  updates!
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import { fetchTaskCard } from "@/api/airdrop";
import navHeader from "./components/nav-header.vue";
export default {
  data() {
    return {
      scrollTop: 0,
      info: {
        avatar: "",
        completed: "-",
        medals: [],
        name: "",
        points: "-",
        rank: "-",
      },
    };
  },
  computed: {
    ...mapState({
      userInfo: (s) => s.userInfo,
    }),
    addr() {
      console.log(this.userInfo);
      if (Object.values(this.userInfo).length > 0) {
        if (this.userInfo.wallet) {
          return (
            this.userInfo.wallet.address.slice(0, 4) +
            "..." +
            this.userInfo.wallet.address.slice(-4)
          );
        } else {
          return this.userInfo.username;
        }
      }
      return "-";
    },
    points() {
      if (Object.values(this.userInfo).length > 0) {
        return this.info.points;
      }
      return "-";
    },
    tasks() {
      if (Object.values(this.userInfo).length > 0) {
        return this.info.completed;
      }
      return "-";
    },
    rank() {
      if (Object.values(this.userInfo).length > 0) {
        return this.info.rank || 0;
      }
      return "-";
    },
    x() {
      return this.info.medals.includes("TWITTER");
    },
    dc() {
      return this.info.medals.includes("DISCORD");
    },
    tg() {
      return this.info.medals.includes("TELEGRAM");
    },
    onChain() {
      return this.info.medals.includes("ON_CHAIN");
    },
  },
  components: {
    navHeader,
  },
  created() {
    this.getInfo();
  },
  methods: {
    onScroll(e) {
      this.scrollTop = e.target.scrollTop;
    },
    handleBnB() {
      this.$router.push("/quest/bnb");
    },

    async getInfo() {
      try {
        if (!localStorage.token) return;
        const { data } = await fetchTaskCard();
        this.info = data;
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.text-white {
  color: #fff;
}
.airdrop {
  height: 100vh;
  margin: 0 auto;
  overflow: scroll;
  background: #111214;

  .airdrop-body {
    max-width: 1440px;
    margin: 0 auto;
    color: #f1f5f9;
    padding: 0px 48px;
    .airdrop-intro {
      padding: 12px 0 64px 0;
      border-bottom: 1px solid #3a3d43;
      .airdrop-desc {
        flex: 1;
        margin-right: 60px;
      }
      .airdrop-card-container {
        position: relative;
        width: 433px;
        .airdrop-card {
          width: 100%;
          position: absolute;
          top: -120px;
          width: 100%;
          height: 200px;
          border-radius: 16px;
          background: url("/img/airDrop/airdrop-card.png") no-repeat;
          background-size: 100% 100%;
          backdrop-filter: blur(10px);

          .card-title {
            font-size: 28px;
            color: #fff;
          }
          .card-avatar {
            object-fit: cover;
            width: 88px;
            height: 88px;
            background: rgba(0, 0, 0, 0.5);
            border: 1px solid #3a3d43;
          }
        }
      }
    }
    .task-list {
      .task-item {
        border-radius: 16px;
        background: #15171a;
        overflow: hidden;
        .task-img-wrap {
          width: 432px;
          background: #1a1c21;
        }
        .task-desc {
          flex: 1;
        }
        .reward {
          color: #ffce56;
        }
      }
    }
  }
}

@media screen and (max-width: 768px) {
  .airdrop-bg {
    margin-top: 54px;

    height: 200px;
    object-fit: cover;
  }
  .airdrop-header {
    padding: 12px 16px !important;
  }
  .airdrop-body {
    padding: 0px 16px !important;
    .airdrop-card-container {
      width: 100% !important;
    }
    .airdrop-desc {
      margin-right: 0 !important;
    }
  }
  .airdrop-intro {
    flex-direction: column;

    padding: 12px 0 !important;
    .airdrop-card {
      margin-top: 16px;
      position: static !important;
    }
  }
  .task-item {
    max-width: 100%;
    flex-direction: column;
    .task-img-wrap {
      width: 100% !important;
      padding: 12px !important;
      img {
        width: 100%;
      }
    }
    .task-desc {
      padding: 12px !important;
    }
  }
}
</style>
