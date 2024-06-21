#!/usr/bin/env python

import rospy
from std_msgs.msg import Int32
from sound_play.libsoundplay import SoundClient
import random

# サウンドファイルのパスをまとめて宣言
start_sound = "/home/takeda-y/work_space/src/daruma_count/ゲームスタート.wav"        # ゲームスタート
daruma1_sound = "/home/takeda-y/work_space/src/daruma_count/だるま.wav"                # だるまさんが転んだ
daruma2_sound = "/home/takeda-y/work_space/src/daruma_count/だるま２.wav"
daruma3_sound = "/home/takeda-y/work_space/src/daruma_count/だるま３.wav"
daruma4_sound = "/home/takeda-y/work_space/src/daruma_count/だるま４.wav"
daruma5_sound = "/home/takeda-y/work_space/src/daruma_count/だるま５.wav"
daruma6_sound = "/home/takeda-y/work_space/src/daruma_count/だるま６.wav"
daruma7_sound = "/home/takeda-y/work_space/src/daruma_count/だるま７.wav"
ten_sound = "/home/takeda-y/work_space/src/daruma_count/１０秒.wav"                   # 残り10秒
five_sound = "/home/takeda-y/work_space/src/daruma_count/５秒.wav"                    # 残り5秒
four_sound = "/home/takeda-y/work_space/src/daruma_count/４秒.wav"                    # 残り4秒
three_sound = "/home/takeda-y/work_space/src/daruma_count/３秒.wav"                   # 残り3秒
two_sound = "/home/takeda-y/work_space/src/daruma_count/２秒.wav"                     # 残り2秒
one_sound = "/home/takeda-y/work_space/src/daruma_count/１秒.wav"                     # 残り1秒
zero_sound = "/home/takeda-y/work_space/src/daruma_count/０秒.wav"                    # 残り0秒
failure_sound = "/home/takeda-y/work_space/src/daruma_count/ゲームオーバー.wav"       # ゲームオーバー
correct_sound = "/home/takeda-y/work_space/src/daruma_count/ピンポン.wav"             # ピンポン
incorrect_sound = "/home/takeda-y/work_space/src/daruma_count/ブッブー.wav"           # ブッブー
success_sound = "/home/takeda-y/work_space/src/daruma_count/ゲームクリア.wav"         # ゲームクリア

sc = None

def start_voice():
    sc.playWave(start_sound)

def daruma_voice(): #ランダムにだるまさんがころんだの音声をだす
    # サウンドファイルのリストを作成
    sound_files = [daruma1_sound, daruma2_sound, daruma3_sound, daruma4_sound, daruma5_sound, daruma6_sound, daruma7_sound]
    selected_sound = random.choice(sound_files) # ランダムにサウンドファイルを選択
    sc.playWave(selected_sound)  # 選択されたサウンドを再生

def ten_seconds_left():
    sc.playWave(ten_sound)

def five_seconds_left():
    sc.playWave(five_sound)

def four_seconds_left():
    sc.playWave(four_sound)

def three_seconds_left():
    sc.playWave(three_sound)

def two_seconds_left():
    sc.playWave(two_sound)

def one_second_left():
    sc.playWave(one_sound)

def zero_seconds_left():
    sc.playWave(zero_sound)
    sc.playWave(failure_sound)

def timer_callback(event):
    total_time = rospy.Duration(20.0)  # 制限時間（2分 = 120秒）
    #event.last_real = 0
    if event.last_real is None:
        event.last_real = rospy.rostime.Time()

    #print(type(event.current_real))
    #print(type(event.last_real))
    #print(type(elapsed_time))
    print("current: " + str(event.current_real))
    print("last: " + str(event.last_real))

    elapsed_time = event.current_real - event.last_real  # 現在の経過時間
    remaining_time = total_time - elapsed_time  # 残り時間
    print("elapsed: " + str(elapsed_time))
    #print(type(event.last_real))

    # 残り時間に応じて関数を呼び出す
    if remaining_time == rospy.Duration(10.0):
        ten_seconds_left()
    elif remaining_time == rospy.Duration(5.0):
        five_seconds_left()
    elif remaining_time == rospy.Duration(4.0):
        four_seconds_left()
    elif remaining_time == rospy.Duration(3.0):
        three_seconds_left()
    elif remaining_time == rospy.Duration(2.0):
        two_seconds_left()
    elif remaining_time == rospy.Duration(1.0):
        one_second_left()
    elif remaining_time == rospy.Duration(0.0):
        zero_seconds_left()
        rospy.signal_shutdown('Lidar detected movement, shutting down.')

def number_input_callback(msg):
    if msg.data == 0:  # 不正解
        rospy.loginfo("number_input incorrect, playing ブッブー.wav, ゲームオーバー.wav")
        sc.playWave(incorrect_sound)
        sc.playWave(failure_sound)
        rospy.signal_shutdown('Lidar detected movement, shutting down.')

    if msg.data == 1:  # 正解
        rospy.loginfo("number_input correct, playing ピンポン.wav, ゲームクリア.wav")
        sc.playWave(correct_sound)
        sc.playWave(success_sound)
        rospy.signal_shutdown('Lidar detected movement, shutting down.')

def detect_human_callback(msg):
    if msg.data == 0:  # 動いたとき
        rospy.loginfo("detect_human 0, playing ゲームオーバー.wav")
        sc.playWave(failure_sound)
        rospy.signal_shutdown('Lidar detected movement, shutting down.')

# def lidar_detect_callback(msg):
#     if msg.data == 0:  # 動いたとき
#         rospy.loginfo("lidar_detect 0, playing ゲームオーバー.wav")
#         sc.playWave(failure_sound)
#         rospy.signal_shutdown('Lidar detected movement, shutting down.')

if __name__ == '__main__':
    rospy.init_node('daruma_count')
    sc = SoundClient()
    
    rospy.Subscriber("number_input", Int32, number_input_callback)
    rospy.Subscriber("detect_human", Int32, detect_human_callback)
    # rospy.Subscriber("lidar_detect", Int32, lidar_detect_callback)

    rospy.sleep(1)  # sound_playノードが起動するのを待つ
    
    rospy.Timer(rospy.Duration(1.0), timer_callback)  # 1秒ごとにタイマーコールバックを呼び出す

    start_voice()  # ゲームスタート
    rospy.loginfo("ゲームスタート")

    rate = rospy.Rate(10)

    rospy.sleep(3.0)
    while not rospy.is_shutdown():
        daruma_voice()  # だるまさんがころんだ
        rospy.sleep(7.0)
        rate.sleep()


