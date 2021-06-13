###Unity Loading Json file contents and image url Unity 2020







using System.Collections;
using UnityEngine.Networking;
using UnityEngine.UI;



using System.Collections.Generic;
using UnityEngine;




public struct Data {
    public string Name;
    public string ImageURL;

}



public class JSON : MonoBehaviour
{
    [SerializeField]
    Text uiNameText;
    [SerializeField]
    RawImage uiRawimage;

    string jsonURL = "https://drive.google.com/uc?export=download&id=1UPL3diHal-cvDm9Zhc-HgjWDV5WmxAcr";


  



    //////////////////////////////////   Load button function  ////////////////////////
    public void loadJson()
    {

        StartCoroutine(GetData(jsonURL));
    }




    //////////////////////////////////   json file loading function ////////////////////////
    IEnumerator GetData(string url)
    {
        UnityWebRequest request = UnityWebRequest.Get(url);
        yield return request.SendWebRequest();



        if (request.result == UnityWebRequest.Result.ConnectionError)
        {
            //error...
        }
        else 
        {
            //success

            //store the DATA struct from json replies
            Data data = JsonUtility.FromJson<Data>(request.downloadHandler.text);

            //print data in UI
            uiNameText.text = data.Name;

            //Load Image from the data replies of json request
            StartCoroutine(GetImage(data.ImageURL));
        
        }
        request.Dispose();
    }






    //////////////////////////////////   image file from json loading function ////////////////////////
    IEnumerator GetImage(string url)
    {
        UnityWebRequest request = UnityWebRequestTexture.GetTexture(url);
        yield return request.SendWebRequest();



        if (request.result == UnityWebRequest.Result.ConnectionError)
        {
            //error...
        }
        else
        {
            //success
            uiRawimage.texture = ((DownloadHandlerTexture)request.downloadHandler).texture;
        
        }

        request.Dispose();

    }









}//end 
