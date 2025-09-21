using UnityEngine;
using System.Collections.Generic;

public enum WeatherType
{
    Clear,
    Rain,
    Thunderstorm,
    Snow,
    Windy
}

public class WeatherSystem : MonoBehaviour
{
    public WeatherType currentWeather;
    public float weatherDuration = 60f; // Duration for each weather type
    private float timer = 0f;

    // References to particle systems and effects
    public ParticleSystem rainParticles;
    public ParticleSystem snowParticles;
    public AudioSource thunderAudio;
    public GameObject windEffect;
    public List<GameObject> puddlePrefabs;

    void Start()
    {
        SetWeather(WeatherType.Clear);
    }

    void Update()
    {
        timer += Time.deltaTime;
        if (timer > weatherDuration)
        {
            WeatherType nextWeather = GetRandomWeather();
            SetWeather(nextWeather);
            timer = 0f;
        }
    }

    WeatherType GetRandomWeather()
    {
        // Custom logic to select weather based on area or randomness
        int rand = Random.Range(0, 5);
        return (WeatherType)rand;
    }

    void SetWeather(WeatherType type)
    {
        currentWeather = type;
        rainParticles.Stop();
        snowParticles.Stop();
        thunderAudio.Stop();
        windEffect.SetActive(false);

        switch (type)
        {
            case WeatherType.Clear:
                // No effects
                break;
            case WeatherType.Rain:
                rainParticles.Play();
                SpawnPuddles();
                break;
            case WeatherType.Thunderstorm:
                rainParticles.Play();
                thunderAudio.Play();
                // Trigger enemy electric boost here
                break;
            case WeatherType.Snow:
                snowParticles.Play();
                break;
            case WeatherType.Windy:
                windEffect.SetActive(true);
                // Affect jump physics here
                break;
        }
    }

    void SpawnPuddles()
    {
        // Example: Spawn puddles in the scene
        foreach (var puddle in puddlePrefabs)
        {
            puddle.SetActive(true);
            // Puddles could use reflective shaders for Knight reflection
        }
    }
}
